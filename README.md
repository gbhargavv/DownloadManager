
Android Http Download Manager
===========================

A valuable and compelling http download director for Android. This download director is planned by the thought and execution of Volley.

Usage
=====
* In the event that you don't set the objective document way, the download supervisor will utilize 'Environment.DIRECTORY_DOWNLOADS' in SDCard as default registry, and it will identify filename consequently from header or url if destinationFilePath not set:
```java
DownloadManager manager = new DownloadManager.Builder().context(this)
        .downloader(OkHttpDownloader.create(client))
        .threadPoolSize(3)
        .logger(new Logger() {
          @Override public void log(String message) {
            Log.d("TAG", message);
          }
        })
        .build();
String destPath = Environment.getExternalStorageDirectory() + File.separator + "test.apk";
DownloadRequest request = 
          new DownloadRequest.Builder()
              .url("http://something.to.download")
              .retryTime(5)
              .retryInterval(2, TimeUnit.SECONDS)
              .progressInterval(1, TimeUnit.SECONDS)
              .priority(Priority.HIGH)
              .allowedNetworkTypes(DownloadRequest.NETWORK_WIFI)
              .destinationFilePath(destPath)
              .downloadCallback(new DownloadCallback() {
                  @Override public void onStart(int downloadId, long totalBytes) {
						
                  }

                  @Override public void onRetry(int downloadId) {
						
                  }

                  @Override
                  public void onProgress(int downloadId, long bytesWritten, long totalBytes) {
						
                  }

                  @Override public void onSuccess(int downloadId, String filePath) {
						
                  }

                  @Override public void onFailure(int downloadId, int statusCode, String errMsg) {
						
                  }
              })
              .build();
				
int downloadId = manager.add(request);
```
It's easy to stop:
```java
	/* stop single */
	manager.cancel(downloadId);
	/* stop all */
	manager.cancelAll();
```

* If you would prefer not to set the filename yet need to set the download catalog, then, at that point you can utilize 'destinationDirectory(String registry)', however this technique will be overlooked if 'destinationFilePath((String filePath)' was utilized. 

* You can likewise set retry time with strategy 'retryTime(int retryTime)' if fundamental, default retry time is 1. You can set retry stretch to choose how long to retry with technique 'retryInterval(long span, TimeUnit unit)'. 

* This administrator support downloading in various organization type with technique 'allowedNetworkTypes(int types)', the sorts can be 'DownloadRequest.NETWORK_MOBILE' and 'DownloadRequest.NETWORK_WIFI'. This strategy need *android.permission.ACCESS_NETWORK_STATE* consent. 

* The string pool size of download supervisor is 3 naturally. In the event that you need a bigger pool, you can attempt the strategy 'threadPoolSize(int poolSize)' in 'DownloadManager#Builder'. 

* You need *android.permission.WRITE_EXTERNAL_STORAGE* consent on the off chance that you don't utilize public catalog in SDCard as download objective record way. Remember to add *android.permission.INTERNET* authorization. 

* This download director support breakpoint downloading, so you can restart the downloading after stop. 

* If you don't need DownloadDispathcer conjure 'onProgress(int downloadId, long bytesWritten, long totalBytes)' as often as possible, then, at that point you can utilize 'progressInterval(long stretch, TimeUnit unit)'. 

* If you need one download demand get high need, then, at that point you can utilize 'priority(Priority need)'. 

* The download supervisor gives two sorts of 'Downloader'('URLDownloader' and 'OkHttpDownloader'), and the it will distinguish which downloader to utilize. You can likewise execute your own 'Downloader' very much like what 'URLDownloader' and 'OkHttpDownloader' do.


Download
========

        implementation 'com.github.gbhargavv:DownloadManager:1.0.2'


Note
====
Assuming you're utilizing 'OkHttpDownloader' with custom 'OkHttpClient' as 'Downloader' in 'DownloadManager', you ought not add [HttpLoggingInterceptor][2] in your custom 'OkHttpClient'. It could be crashed(OOM) as 'HttpLoggingInterceptor ' use 'okio' to reqeust the entire body in memory.

Credits
=======
* [Volley][1] - Google networking library for android.

