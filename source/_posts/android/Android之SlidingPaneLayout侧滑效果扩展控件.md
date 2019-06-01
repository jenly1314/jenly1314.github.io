---
title: Android之SlidingPaneLayout侧滑效果扩展（SuperSlidingPaneLayout）
date: 2016-10-08 17:06:59
img: images/20161008.jpg
categories: Android
tags:
  - SlidingPaneLayout
  - 仿QQ侧滑效果
  - Android
---

### 前言：
说到侧滑菜单，记得在很久很久以前，一说到侧滑菜单就会立刻想到SlidingMenu，在当时的印象里比较火的侧滑菜单就是SlidingMenu，最开始觉得那种效果还蛮新颖的，后来Google官方出了SlidingPaneLayout和DrawerLayout后，大部分的侧滑菜单效果也就基本被满足了。本博文主要讲到基于官方v4扩展包中的SlidingPaneLayout来扩展侧滑效果，我给SldingPaneLayout的扩展控件取了个好听的名字叫：**SuperSlidingPaneLayout**

> 说到侧滑效果扩展，这里主要用到了平移、缩放、等效果组合来达到想要的效果。

### 首先:
我们在**SuperSlidingPaneLayout**中定义一个枚举来表示不同的侧滑效果模式：
```java
public enum Mode {
 
        DEFAULT(0),
        TRANSLATION(1),
        SCALE_MENU(2),
        SCALE_PANEL(3),
        SCALE_BOTH(4),
        TRANSLATION_SCALE(5);
 
        private int mValue;
 
        Mode(int value){
            this.mValue = value;
        }
 
        private static Mode getFromInt(int value){
 
            for(Mode mode : Mode.values()){
                if(mode.mValue == value)
                    return mode;
            }
 
            return DEFAULT;
        }
    }

```
### 接着:
我们可以在SlidingPaneLayout源码中找到一个dispatchOnPanelSlide方法，其代码如下：
```java
  void dispatchOnPanelSlide(View panel) {
        if (mPanelSlideListener != null) {
            mPanelSlideListener.onPanelSlide(panel, mSlideOffset);
        }
    }

```
### 思考：
从方法字面意思我们不难看出这是一个调度面板滑动的方法，说白了就是里面就是一个面板滑动的监听事件，我们可以通过(slidingPaneLayout.setPanelSlideListener)设置监听来监听面板的滑动情况，既然是扩展，就不想影响监听事件，这里我们在SuperSlidingPaneLayout中新增一个方法，代码如下：
```java
    private void dispatchOnPanelMode(View panel, float slideOffset){
        if (mMenuPanel == null) {
            final int childCount = getChildCount();
            for (int i = 0; i < childCount; i++) {
                final View child = getChildAt(i);
                if (child != panel) {
                    mMenuPanel = child;
                    break;
                }
            }
        }
        final float scale = 1 - 0.2f * slideOffset;
        final float scaleLeft = 1 - 0.2f * (1 - slideOffset);
        switch (mMode){
            case TRANSLATION://平移效果
                mMenuPanel.setTranslationX((slideOffset-1) * mMenuPanel.getWidth());
                panel.setScaleX(1);
                panel.setScaleY(1);
 
                break;
            case SCALE_MENU://缩放菜单效果
                mMenuPanel.setPivotX(-0.2f * mMenuPanel.getWidth());
                mMenuPanel.setPivotY(mMenuPanel.getHeight() / 2.0f);
                mMenuPanel.setScaleX(scaleLeft);
                mMenuPanel.setScaleY(scaleLeft);
                panel.setScaleX(1);
                panel.setScaleY(1);
 
                break;
            case SCALE_PANEL://缩放面板效果
                panel.setPivotX(0);
                panel.setPivotY(panel.getHeight()/2.0f);
                panel.setScaleX(scale);
                panel.setScaleY(scale);
 
                break;
            case SCALE_BOTH://菜单面板都缩放效果
                mMenuPanel.setPivotX(-0.2f * mMenuPanel.getWidth());
                mMenuPanel.setPivotY(mMenuPanel.getHeight() / 2.0f);
                mMenuPanel.setScaleX(scaleLeft);
                mMenuPanel.setScaleY(scaleLeft);
 
                panel.setPivotX(0);
                panel.setPivotY(panel.getHeight()/2.0f);
                panel.setScaleX(scale);
                panel.setScaleY(scale);
 
                break;
            case TRANSLATION_SCALE://平移缩放效果
                mMenuPanel.setTranslationX((slideOffset-1) * mMenuPanel.getWidth() / 2.0f);
                mMenuPanel.setPivotX(-0.2f * mMenuPanel.getWidth());
                mMenuPanel.setPivotY(mMenuPanel.getHeight() / 2.0f);
                mMenuPanel.setScaleX(scaleLeft);
                mMenuPanel.setScaleY(scaleLeft);
 
                panel.setPivotX(0);
                panel.setPivotY(panel.getHeight()/2.0f);
                panel.setScaleX(scale);
                panel.setScaleY(scale);
 
                break;
            case DEFAULT://默认 即菜单固定
            default:
                panel.setPivotX(0);
                panel.setPivotY(0);
                panel.setScaleX(1);
                panel.setScaleY(1);
 
                break;
        }
    }
```
### 解读：
在这里简单说明下上面代码的意思，上面代码逻辑主要根据面板的偏移量来进行缩放、平移面板或菜单，达到不同的侧滑效果。
### 然后：
在**dispatchOnPanelSlide**中调用一下就可以了，代码如下：
```java
    void dispatchOnPanelSlide(View panel) {
        dispatchOnPanelMode(panel,mSlideOffset);
        if (mPanelSlideListener != null) {
            mPanelSlideListener.onPanelSlide(panel, mSlideOffset);
        }
    }
```
### 收工：
这样几种不同的侧滑效果就算完成了，有木有很简单？下面我们来看下最终的效果图：
![](Android之SlidingPaneLayout侧滑效果扩展/20161008172543455.gif)

[SuperSlidingPaneLayout](https://github.com/jenly1314/SuperSlidingPaneLayout)最新源码已上传至**github**欢迎**Star**和**Fork**。
