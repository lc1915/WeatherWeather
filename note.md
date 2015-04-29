#Note
###关于沉浸模式中几个参数
https://developer.android.com/training/system-ui/immersive.html#nonsticky 

Android 4.4 (API Level 19) introduces a new SYSTEM_UI_FLAG_IMMERSIVE flag for setSystemUiVisibility() that lets your app go truly "full screen." This flag, when combined with the SYSTEM_UI_FLAG_HIDE_NAVIGATION and SYSTEM_UI_FLAG_FULLSCREEN flags, hides the navigation and status bars and lets your app capture all touch events on the screen.

It's good practice to include other system UI flags (such as SYSTEM_UI_FLAG_<b>LAYOUT</b>_HIDE_NAVIGATION and SYSTEM_UI_FLAG_<b>LAYOUT</b>_STABLE) to keep the content from resizing(调整) when the system bars hide and show. You should also make sure that the action bar and other UI controls are hidden at the same time. This snippet demonstrates how to hide and show the status and navigation bars, without resizing the content:
```java
// This snippet hides the system bars.
private void hideSystemUI() {
    // Set the IMMERSIVE flag.
    // Set the content to appear under the system bars so that the content
    // doesn't resize when the system bars hide and show.
    mDecorView.setSystemUiVisibility(
            View.SYSTEM_UI_FLAG_LAYOUT_STABLE
            | View.SYSTEM_UI_FLAG_LAYOUT_HIDE_NAVIGATION // 如果不要这一行以及下面那一行，在应用打开的时候，界面有一个瞬间的跳动，界面中的所有layout都有一个根据屏幕移动位置的短暂过程。如果加上这两行的话，在一开始加载的时候就按照fullscreen加载，就不会出现那种情况
            | View.SYSTEM_UI_FLAG_LAYOUT_FULLSCREEN
            | View.SYSTEM_UI_FLAG_HIDE_NAVIGATION // hide nav bar
            | View.SYSTEM_UI_FLAG_FULLSCREEN // hide status bar
            | View.SYSTEM_UI_FLAG_IMMERSIVE);
}

// This snippet shows the system bars. It does this by removing all the flags
// except for the ones that make the content appear under the system bars.
private void showSystemUI() {
    mDecorView.setSystemUiVisibility(
            View.SYSTEM_UI_FLAG_LAYOUT_STABLE
            | View.SYSTEM_UI_FLAG_LAYOUT_HIDE_NAVIGATION
            | View.SYSTEM_UI_FLAG_LAYOUT_FULLSCREEN);
}
```
##Android View的绘制流程
Drawing the layout is a two pass process: a **measure** pass and a **layout** pass. The measuring pass is implemented in `measure(int, int)` and is a **top-down traversal of the View tree**. Each View pushes dimension specifications down the tree during the recursion. At the end of the measure pass, every View has stored its measurements. The second pass happens in `layout(int, int, int, int)` and is also top-down. During this pass each parent is responsible for positioning all of its children using the sizes computed in the measure pass.

##remain to be done
getWindow().getDecorView()
