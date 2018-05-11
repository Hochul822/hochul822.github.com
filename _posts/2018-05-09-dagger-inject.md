---
layout: post
title : "[Dagger] 안드로이드 대거 - 시작"
date: 2018-05-09 10:00:00
image :
'https://firebasestorage.googleapis.com/v0/b/hellofirebase-373d3.appspot.com/o/tech-blog%2FAndroid-dagger-2.png?alt=media&token=18d56a3c-c4d7-4f90-a90a-c094e8d16c36'
category: 'blog'
tags:
- app
- android
- dagger
twitter_text: Lorem ipsum dolor sit amet, consectetur adipisicing elit.
introduction: Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.
---

## @Inject, @Component

## code

```kt

class Info constructor() {
    var text = "Hello Dagger"
}

class MainActivity : AppCompatActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        val info = Info()
        textview.text = info.text
    }
}

```
