---
title: GestureDetector调节音量和亮度
date: 2016-09-18 12:57:26
tags: 
- Gesture
- Android
categories: 
- 移动开发
---

> 0918更新

# 效果
手势调节音量和屏幕亮度
横屏屏幕左侧调节音量，右侧调节屏幕亮度
# 方法
继承GestureDetector.OnGestureListener，覆盖对应的手势方法
<!--more-->
## onFling|onScroll方法参数解释
e1：第1个ACTION_DOWN MotionEvent
e2：最后一个ACTION_MOVE MotionEvent
velocityX：X轴上的移动速度，像素/秒
velocityY：Y轴上的移动速度，像素/秒

## onFling和onScroll的的区别
### 滑动距离上
二者没有（明显）区别，在检测效果的操作中，滑动距离无论长短都会触发。

### 滑动速率、触发顺序上
有区别： 
onFling（）为“滑动”的最后触发（即手指Up抬起时触发），需要较为快速的"滑动"操作（但在"滑动"过程中，也会不停的触发onScroll（）），如果慢速滑动，通过日志可以看出，最后没有调用onFling()。
                
onScroll（）为“拖动”或“滑动”的过程中不断触发,直到动作结束，无论快慢都会触发。

## GestureDetector.OnGestureListener
```java
public class GestureDetectorListener implements GestureDetector.OnGestureListener {
    private float mDownY;
    public PlayerGestureDetectorListener(Context context) {
        this.context = context;
    }

    @Override
    public boolean onDown(MotionEvent e) {
        @Override
        public boolean onDown(MotionEvent e) {
            mDownY = e.getY();
        }
    }

    @Override
    public boolean onScroll(MotionEvent e1, MotionEvent e2, float velocityX, float velocityY) {
        if (context.getResources().getConfiguration().orientation == Configuration.ORIENTATION_PORTRAIT) {
            return true;
        }
        if (Math.abs(velocityX) > Math.abs(velocityY)) {
            return false;
        }
        float moveY = e2.getY();
        float distance = moveY - mDownY;
        float scroll_min_velocity = context.getResources()
                .getDimension(R.dimen.p_10);
        if (ScreenUtils.isInLeft(context, e1.getX())) {
            if (distance < 0 && Math.abs(distance) > scroll_min_velocity) {
                increaseLight();
                mDownY = moveY;
            } else if (distance > 0 && distance > scroll_min_velocity) {
                decreaseLight();
                mDownY = moveY;
            }
        } else {
            if (distance < 0 && Math.abs(distance) > scroll_min_velocity) {
                increaseVoice(Math.abs(distance));
                mDownY = moveY;
            } else if (distance > 0 && distance > scroll_min_velocity) {
                decreaseVoice(distance);
                mDownY = moveY;
            }
        }
        return false;
    }

    private void increaseLight() {
        applySystemPermission();
        float mBrightness = ((Activity) context).getWindow().getAttributes().screenBrightness;
        if (mBrightness <= 0.00f)
            mBrightness = 0.50f;

        WindowManager.LayoutParams lpa = ((Activity) context).getWindow().getAttributes();
        lpa.screenBrightness = mBrightness + 0.01f;
        if (lpa.screenBrightness > 1.0f) {
            lpa.screenBrightness = 1.0f;
        }
        ((Activity) context).getWindow().setAttributes(lpa);
    }

    private void decreaseLight() {
        applySystemPermission();
        float mBrightness = ((Activity) context).getWindow().getAttributes().screenBrightness;
        if (mBrightness < 0.01f)
            mBrightness = 0.01f;
        WindowManager.LayoutParams lpa = ((Activity) context).getWindow().getAttributes();
        lpa.screenBrightness = mBrightness - 0.01f;
        if (lpa.screenBrightness < 0.01f)
            lpa.screenBrightness = 0.01f;
        ((Activity) context).getWindow().setAttributes(lpa);
    }

    private void increaseVoice(float y) {
        AudioManager audiomanager = (AudioManager) context.getSystemService(Context.AUDIO_SERVICE);
        int maxVolume = audiomanager.getStreamMaxVolume(AudioManager.STREAM_MUSIC); // 获取系统最大音量
        int currentVolume = audiomanager.getStreamVolume(AudioManager.STREAM_MUSIC); // 获取当前值
        if (currentVolume < maxVolume) {
            currentVolume += y / 10;
            if (currentVolume >= maxVolume) {
                currentVolume = maxVolume;
            }
        }
        //第三个参数FLAG_SHOW_UI，显示系统的音量条
        audiomanager.setStreamVolume(AudioManager.STREAM_MUSIC, currentVolume, AudioManager.FLAG_SHOW_UI);
    }

    private void decreaseVoice(float y) {
        AudioManager audiomanager = (AudioManager) context.getSystemService(Context.AUDIO_SERVICE);
        int currentVolume = audiomanager.getStreamVolume(AudioManager.STREAM_MUSIC); // 获取当前值
        if (currentVolume > 0) {
            currentVolume -= y / 10;
            if (currentVolume < 0) {
                currentVolume = 0;
            }
        }
        audiomanager.setStreamVolume(AudioManager.STREAM_MUSIC, currentVolume, AudioManager.FLAG_SHOW_UI);
    }

    //增加Android6.0及以上需要进行权限判断
    private void applySystemPermission() {
        // Android 6.0以上机型必须先请求权限
        if (Build.VERSION.SDK_INT >= 23 && !Settings.System.canWrite(context.getApplicationContext())) {
            Intent intent = new Intent(android.provider.Settings.ACTION_MANAGE_WRITE_SETTINGS);
            intent.setData(Uri.parse("package:" + context.getPackageName()));
            intent.addFlags(Intent.FLAG_ACTIVITY_NEW_TASK);
            context.startActivity(intent);
            return;
        }
    }
}
```

## 判断屏幕方向的辅助类
```
public class ScreenUtils {
    //获取屏幕的宽度（像素数）
    public static int getWidth(Context mContext) {
        DisplayMetrics dm = new DisplayMetrics();
        ((Activity) mContext).getWindowManager().getDefaultDisplay()
                .getMetrics(dm);

        return dm.widthPixels;
    }

    public static int getHeight(Context mContext) {
        DisplayMetrics dm = new DisplayMetrics();
        ((Activity) mContext).getWindowManager().getDefaultDisplay()
                .getMetrics(dm);
        int screenHeight = dm.heightPixels;
        return screenHeight;
    }

    public static boolean isInRight(Context mContext, float xWeight) {
        return (xWeight > getWidth(mContext) * 1 / 2);
    }

    //根据当前的位置来进行判断
    public static boolean isInLeft(Context mContext, float xWeight) {
        return (xWeight < getWidth(mContext) * 1 / 2);
    }

}
```
