## Caffeine简介
Caffeine是一个Java高性能的本地缓存库。
实际上，Caffeine这样的本地缓存和ConcurrentMap很像——支持并发，并且支持O(1)时间复杂度的数据存取。二者的主要区别在于：
* ConcurrentMap将存储所有存入的数据，直到你显式将其移除；
* Caffeine将通过给定的配置，自动移除“不常用”的数据，以保持内存的合理占用。
因此，一种更好的理解方式是：Cache是一种带有存储和移除策略的Map。


## 在Java中使用Caffeine
```
<dependency>
    <groupId>com.github.ben-manes.caffeine</groupId>
    <artifactId>caffeine</artifactId>
    <version>2.7.0</version>
</dependency>
```

### Cache
在获取缓存值时，如果想要在缓存值不存在时，原子地将值写入缓存，则可以调用get(key, k -> value)方法，该方法将避免写入竞争。
调用invalidate()方法，将手动移除缓存。
多线程情况下，当使用get(key, k -> value)时，如果有另一个线程同时调用本方法进行竞争，则后一线程会被阻塞，直到前一线程更新缓存完成；而若另一线程调用getIfPresent()方法，则会立即返回null，不会被阻塞。


```
Cache<String, String> cache = Caffeine.newBuilder().build();

cache.getIfPresent("123");    // null
cache.get("123", k -> "456"); // 456
cache.getIfPresent("123");    // 123
cache.set("123", "789");
cache.getIfPresent("123");    // 789
```


### LoadingCache
LoadingCache是一种自动加载的缓存。其和普通缓存不同的地方在于，当缓存不存在/缓存已过期时，若调用get()方法，则会自动调用CacheLoader.load()方法加载最新值。调用getAll()方法将遍历所有的key调用get()，除非实现了CacheLoader.loadAll()方法。
使用LoadingCache时，需要指定CacheLoader，并实现其中的load()方法供缓存缺失时自动加载。
多线程情况下，当两个线程同时调用get()，则后一线程将被阻塞，直至前一线程更新缓存完成。


```
LoadingCache<String, String> cache = Caffeine.newBuilder()
        .build(new CacheLoader<String, String>() {
            @Override
            public String load(@NonNull String k) throws Exception {
                return "value1";
            }
            
            @Override        
            public @NonNull Map<String, String> loadAll(@NonNull Iterable<? extends String> keys) throws Exception {
                return null;
            }
        });

cache.getIfPresent("abc"); // null
cache.get("abc");          // value1
cache.getAll(keys);        // Map<String, String>
```

### AsyncCache
AsyncCache是Cache的一个变体，其响应结果均为CompletableFuture，通过这种方式，AsyncCache对异步编程模式进行了适配。默认情况下，缓存计算使用ForkJoinPool.commonPool()作为线程池，如果想要指定线程池，则可以覆盖并实现Caffeine.executor(Executor)方法。
多线程情况下，当两个线程同时调用get(key, k -> value)，则会返回同一个CompletableFuture对象。由于返回结果本身不进行阻塞，可以根据业务设计自行选择阻塞等待或者非阻塞。


```
AsyncCache<String, String> cache = Caffeine.newBuilder().buildAsync();

CompletableFuture<String> completableFuture = cache.get(key, k -> "abc");
completableFuture.get(); // 阻塞，直至缓存更新完成
```


### AsyncLoadingCache
显然这是Loading Cache和Async Cache的功能组合。AsyncLoadingCache支持以异步的方式，对缓存进行自动加载。
类似LoadingCache，同样需要指定CacheLoader，并实现其中的load()方法供缓存缺失是自动加载，该方法将自动在ForkJoinPool.commonPool()线程池中提交。如果想要指定Executor，则可以实现AsyncCacheLoader().asyncLoad()方法。

```
AsyncLoadingCache<String, String> cache = Caffeine.newBuilder()
        .buildAsync(new AsyncCacheLoader<String, String>() {
            @Override
            // 自定义线程池加载
            public @NonNull CompletableFuture<String> asyncLoad(@NonNull String key, @NonNull Executor executor) {
                return null;
            }
        })
        .buildAsync(new CacheLoader<String, String>() {
            @Override
            // OR，使用默认线程池加载（二者选其一）
            public String load(@NonNull String key) throws Exception {
                return "abc";
            }
        });

CompletableFuture<String> completableFuture = cache.get(key); // CompletableFuture<String>
completableFuture.get(); // 阻塞，直至缓存更新完成
```


### 驱逐策略/刷新机制
而使用刷新机制refreshAfterWrite()，Caffeine将在key允许刷新后的首次访问时，立即返回旧值，同时异步地对缓存值进行刷新，这使得调用方不至于因为缓存驱逐而被阻塞。需要注意的是，刷新机制只支持LoadingCache和AsyncLoadingCache。
Automatic refreshes are performed when the first stale request for an entry occurs. The request triggering refresh will make an asynchronous call to CacheLoader.reload and immediately return the old value.


```
Caffeine.newBuilder().maximumSize(1000).build(); // 创建一个最大容量为1000的缓存
Caffeine.newBuilder().expireAfterWrite(10, TimeUnit.HOURS).build(); // 创建一个写入10h后过期的缓存
Caffeine.newBuilder().expireAfterAccess(1, TimeUnit.HOURS).build(); // 创建一个访问1h后过期的缓存

Caffeine.newBuilder().refreshAfterWrite(10, TimeUnit.MINUTES).build(k -> create(k));
```


## 在SpringBoot中使用Caffeine
```
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-cache</artifactId>
</dependency>
```


添加CacheManager

```
@Bean
public CacheManager cacheManager() {
    SimpleCacheManager cacheManager = new SimpleCacheManager();
    ArrayList<CaffeineCache> caches = new ArrayList<>();
    caches.add(new CaffeineCache("cacheName", generateCache()));
    cacheManager.setCaches(caches);
    return cacheManager;
}
```


直接在方法上加@Cacheable 相关注解， 将缓存逻辑跟业务逻辑分开。
和@Cacheable相关的常用的注解包括：
● @Cacheable：表示该方法支持缓存。当调用被注解的方法时，如果对应的键已经存在缓存，则不再执行方法体，而从缓存中直接返回。当方法返回null时，将不进行缓存操作。
● @CachePut：表示执行该方法后，其值将作为最新结果更新到缓存中。每次都会执行该方法。
● @CacheEvict：表示执行该方法后，将触发缓存清除操作。
● @Caching：用于组合前三个注解，例如：


```
@Caching(cacheable = @Cacheable("users"),
         evict = {@CacheEvict("cache2"), @CacheEvict(value = "cache3", allEntries = true)})
public User find(Integer id) {
    return null;
}
```


@Cacheable常用的注解属性如下：
* cacheNames/value：缓存组件的名字，即cacheManager中缓存的名称。
* key：缓存数据时使用的key。默认使用方法参数值，也可以使用SpEL表达式进行编写。
* keyGenerator：和key二选一使用。
* cacheManager：指定使用的缓存管理器。
* condition：在方法执行开始前检查，在符合condition的情况下，进行缓存
* unless：在方法执行完成后检查，在符合unless的情况下，不进行缓存
* sync：是否使用同步模式。若使用同步模式，在多个线程同时对一个key进行load时，其他线程将被阻塞。
下面是一个注解使用示例：
```
@Cacheable(value = "UnitCache",
        key = "#unitType + T(top.kotoumi.constants.Constants).SPLIT_STR + #unitId",
        condition = "#unitType != 'weapon'")
public Unit getUnit(String unitType, String unitId) {
    return getUnit(unitType, unitId);
}
```
