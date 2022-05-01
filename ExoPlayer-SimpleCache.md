# ExoPlayer

# [ExoPlayer](https://exoplayer.dev) Cache

## Architecture

ExoPlayer에서 비디오를 캐싱하고 Player를 통해 로드하고 재생하는 과정입니다.
<img width="1922" alt="exoplayer" src="https://user-images.githubusercontent.com/37677444/166139560-d1169cd0-b9a1-4a75-bdfd-7f735c6a511b.png">


<br>

<br>

<br>

## [SimpleCache](https://exoplayer.dev/doc/reference/com/google/android/exoplayer2/upstream/cache/SimpleCache.html)

[Cache](https://exoplayer.dev/doc/reference/com/google/android/exoplayer2/upstream/cache/Cache.html) 인터페이스를 구현하여 만들어진 객체입니다.

- 지정된 경로에 대해 한번에 하나의 SimpleCache 객체만 허용됩니다.
- SimpleCache를 제거하는 경우, 관련 인덱스 데이터 또한 제거하기 위해 SimpleCache 객체에서 제공되는 delete(File, DatabaseProvider) 메소드를 사용하는 것이 좋습니다.

### 생성자

```java
public SimpleCache(File cacheDir,
                   CacheEvictor evictor,
                   DatabaseProvider databaseProvider)
```

- File cacheDir

SimpleCache 경로를 설정하는 인자입니다.

- [CacheEvictor](https://exoplayer.dev/doc/reference/com/google/android/exoplayer2/upstream/cache/CacheEvictor.html) evictor

캐시에서 데이터를 제거하는 인터페이스로, 구현체는 [LeastRecentlyUsedCacheEvictor](https://exoplayer.dev/doc/reference/com/google/android/exoplayer2/upstream/cache/LeastRecentlyUsedCacheEvictor.html)와 [NoOpCacheEvictor](https://exoplayer.dev/doc/reference/com/google/android/exoplayer2/upstream/cache/NoOpCacheEvictor.html)가 있습니다. LeastRecentlyUsedCacheEvictor는 가장 최근에 사용된 캐싱 파일을 먼저 제거합니다. NoOpCacheEvictor는 캐싱 파일을 제거하지 않습니다. NoOpCacheEvictor를 사용하는 경우 캐시 사이즈 관리가 필요합니다.

- [DatabaseProvider](https://exoplayer.dev/doc/reference/com/google/android/exoplayer2/database/DatabaseProvider.html) databaseProvider

ExoPlayer가 사용할 수 있는 SQLiteDatabase 객체를 제공하는 인터페이스로, 구현체는 [DefaultDatabaseProvider](https://exoplayer.dev/doc/reference/com/google/android/exoplayer2/database/DefaultDatabaseProvider.html)와 [ExoDatabaseProvider](https://exoplayer.dev/doc/reference/com/google/android/exoplayer2/database/ExoDatabaseProvider.html)가 있습니다.

<br>

<br>

<br>

## [CacheDataSource](https://exoplayer.dev/doc/reference/com/google/android/exoplayer2/upstream/cache/CacheDataSource.html)

[DataSource](https://exoplayer.dev/doc/reference/com/google/android/exoplayer2/upstream/DataSource.html)의 구현체로서, 캐시를 읽고 쓰는 객체입니다. 가능한 경우 캐시에서 요청들이 수행되고, 캐시되지 않은 데이터를 읽는 경우에는 업스트림 데이터 소스로 요청하여 캐시에 기록합니다.

```kotlin
private val cacheDataSourceFactory: CacheDataSource by lazy {
        CacheDataSource.Factory()
							// 사용될 캐시 설정 메소드
            .setCache(simpleCache)
							// 캐시가 없을 경우 데이터를 읽어올 소스 설정 메소드
            .setUpstreamDataSourceFactory(httpDataSourceFactory)
							// 데이터 소스 객체 생성 메소드
            .createDataSource()
}
```

위와 같이 캐시 데이터 소스를 생성하여 사용가능합니다. Asset Store 비디오의 경우 업스트림 데이터 소스는 [DefaultHttpDataSource](https://exoplayer.dev/doc/reference/com/google/android/exoplayer2/upstream/DefaultHttpDataSource.html)(HttpUrlConnection을 사용하는 [HttpDataSource](https://exoplayer.dev/doc/reference/com/google/android/exoplayer2/upstream/HttpDataSource.html))를 사용합니다.

<br>

<br>

<br>

## [CacheWriter](https://exoplayer.dev/doc/reference/com/google/android/exoplayer2/upstream/cache/CacheWriter.html)

캐싱 관련 유틸리티 메소드 모음 객체입니다.

### 생성자

```java
public CacheWriter(CacheDataSource dataSource,
                   DataSpec dataSpec,
                   @Nullable
                   byte[] temporaryBuffer,
                   @Nullable
                   CacheWriter.ProgressListener progressListener)
```

- CacheDataSource dataSource

캐싱 되어질 대상입니다.

- DataSpec dataSpec

작성할 데이터가 정의되어있는 객체입니다.

- byte[] temporaryBuffer

캐싱 중 사용할 임시버퍼로, 인자로 null을 사용하면 기본 버퍼(128 * 1024 사이즈의 바이트 배열)를 사용합니다.

- CacheWriter.ProgressLIstener progressListener

캐시 진행상황을 콜백해주는 리스너입니다.

### cache() 메소드

```java
@WorkerThread public void cache() throws IOException
```

데이터 캐시를 요청하고, 이미 캐시 되어있다면 건너뜁니다.
