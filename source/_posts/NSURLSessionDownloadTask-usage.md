---
title: NSURLSessionDownloadTask后台下载遇到的几种情况
date: 2016-06-13 23:22:14
categories:
tags:
---

<!--more-->


1、下载过程中如果直接crash，会在后台继续下载，下载完所有任务后，自动唤醒app，并依次调用：
application:didFinishLaunchingWithOptions:
application:handleEventsForBackgroundURLSession:completionHandler:
// 有几个任务调用几次下面两个方法
URLSession:downloadTask:didFinishDownloadingToURL: // 多次
URLSession:task:didCompleteWithError:   // 多次

URLSessionDidFinishEventsForBackgroundURLSession:
如果在下载完之前，进入前台，会继续下载，下载完的调用下面两个方法：
URLSession:downloadTask:didFinishDownloadingToURL:
URLSession:task:didCompleteWithError:


2、下载过程中直接kill掉app，会在后台重新唤醒，并cancel所有task，并调用：
application:didFinishLaunchingWithOptions:
application:handleEventsForBackgroundURLSession:completionHandler: // 多次

URLSession:task:didCompleteWithError:（可以保存下载中的resumedata） // 多次
URLSessionDidFinishEventsForBackgroundURLSession:
退出前应对未下载的数据进行保存

3、suspend所有task，crash后没有任何反应，重启后可以调用：
getTasksWithCompletionHandler
获得所有downloadTask然后调用resume继续下载

4、suspend所有task后，kill掉app，会在后台唤醒app，然后调用：
application:didFinishLaunchingWithOptions:
application:handleEventsForBackgroundURLSession:completionHandler: // 次数=task.count

URLSession:task:didCompleteWithError:（可以保存下载中的resumedata）  //次数=task.count
URLSessionDidFinishEventsForBackgroundURLSession:

4、下载过程中cancel，调用:
URLSession:task:didCompleteWithError：
然后从session中移除

5、下载过程中，退出app到后台，所有任务下载完后，调用
application:handleEventsForBackgroundURLSession:completionHandler:

URLSession:downloadTask:didFinishDownloadingToURL:   // 多次
URLSession:task:didCompleteWithError:				   // 多次

URLSessionDidFinishEventsForBackgroundURLSession:
如果进入前台时，任务还未下完，会继续下载
