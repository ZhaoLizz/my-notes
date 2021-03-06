# 通知

## 1.创建通知

1. 通过系统服务方法获取通知manager
2. 通过一个兼容版本的通知类的构建器构建通知
3. manager.notify(..)

```java
public void onClick(View v) {
       switch (v.getId()){
           case R.id.send_notice:
               NotificationManager manager = (NotificationManager)getSystemService(NOTIFICATION_SERVICE);
               //创建兼容版本的通知
               Notification notification = new NotificationCompat.Builder(this)
                       .setContentTitle("Title")
                       .setContentText("Text")
                       .setWhen(System.currentTimeMillis())
                       //设置小图标
                       .setSmallIcon(R.mipmap.ic_launcher)
                       .setLargeIcon(BitmapFactory.decodeResource(getResources(),R.mipmap.ic_launcher))
                       .build();
               //第一个参数int id，要保证每个通知的id不同
               manager.notify(1,notification);
       }
   }
```

```java
//构建Notification
            Resources resources = getResources();
            Intent i = PhotoGalleryActivity.newIntent(this);
            PendingIntent pi = PendingIntent.getActivity(this, 0, i, 0);

            Notification notification = new NotificationCompat.Builder(this)
                    //TickerText
                    .setTicker(resources.getString(R.string.new_pictures_title))
                    .setSmallIcon(android.R.drawable.ic_menu_report_image)
                    //标题
                    .setContentTitle(resources.getString(R.string.new_pictures_title))
                    //显示文字
                    .setContentText(resources.getString(R.string.new_pictures_text))
                    //点击事件
                    .setContentIntent(pi)
                    .setAutoCancel(true)
                    .build();

            NotificationManagerCompat notificationManager =
                    NotificationManagerCompat.from(this);
            //唯一的参数id.如果使用同一id,第二个会覆盖第一个通知
            notificationManager.notify(0, notification);
        }
```

## 2.创建通知点击事件

1. 利用PendingIntent.getActivity 把一个intent包装为PendingIntent延迟意图
2. 在通知的Builder下连缀setContentIntent、setAutoCancel

```java
public void onClick(View v) {
        switch (v.getId()){
            case R.id.send_notice:

                Intent intent = new Intent(this,NotificationActivity.class);
                //把intent包装为PendingIntent延迟意图
                PendingIntent pendingIntent = PendingIntent.getActivity(this,0,intent,0);


                NotificationManager manager = (NotificationManager)getSystemService(NOTIFICATION_SERVICE);
                //创建兼容版本的通知
                Notification notification = new NotificationCompat.Builder(this)
                        .setContentTitle("Title")
                        .setContentText("Text")
                        .setWhen(System.currentTimeMillis())
                        //设置小图标
                        .setSmallIcon(R.mipmap.ic_launcher)
                        .setLargeIcon(BitmapFactory.decodeResource(getResources(),R.mipmap.ic_launcher))

                        .setContentIntent(pendingIntent)
                        .setAutoCancel(true)

                        .build();
                //第一个参数int id，要保证每个通知的id不同
                manager.notify(1,notification);
        }
    }
```

## 3.通知的多媒体功能

- 在构建通知的Builder里连缀
- 可以使用默认效果 setDefaults(NotificationCompat.DEFAULT_ALL)

  ```java
  .setSound(Uri.fromFile(new File("/system.media/audio/ringtones/luna.ogg")))
                        //设置震动(需要权限VIBRATE)，静、动、静、动
                        .setVibrate(new long[]{0,1000,1000,1000,1000,1000})
                        //亮、暗
                        .setLights(Color.RED,1000,1000)
                        //有五个优先等级常量
                        .setPriority(NotificationCompat.PRIORITY_MAX)
  ```
