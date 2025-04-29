```json
{
  "textList": [
    "对不起，小狗知道错了",
    "以后好吃的都给你",
    "你在玩欲擒故纵吗?",
    "不许点这里",
    "原谅小狗好不好"
  ],
  "imgList": [
    "http://activity.thinkfont.com/rdlove/images/sorry.png",
    "http://activity.thinkfont.com/rdlove/images/please.png",
    "http://activity.thinkfont.com/rdlove/images/ask.png",
    "http://activity.thinkfont.com/rdlove/images/angry.png",
    "http://activity.thinkfont.com/rdlove/images/beg.png"
  ],
  "lastImg": "http://activity.thinkfont.com/rdlove/images/happy.png",
  "lastContent": "汪！狗狗就知道"
}
```

依靠上面的数据
做一个祈求和好的小网站

背景 #f1d5da 粉色

中间是
![imgList\[0\]](http://activity.thinkfont.com/rdlove/images/default.png) 图片
下面是“可以跟小狗和好吗？”这句话
下面的是“和好”粉色的按钮，与“不要 ”蓝色的按钮
如果拒绝
就是依次向下替换图片imgList[0]与蓝色的按钮的文字textList[0]，
直至最后一张图与文字。
但凡点“和好”就替换为 lastContent 与lastImg图片
每次点拒绝，和好的按钮就变大
