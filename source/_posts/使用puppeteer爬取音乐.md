---
title: 使用puppeteer爬取音乐
date: 2020-02-07 14:07:33
tags: [爬虫, puppeteer]
comments: true
categories:
  - 爬虫
---

# 项目地址
## 工具
[puppeteer](https://github.com/puppeteer/puppeteer)
[puppeteer中文](https://zhaoqize.github.io/puppeteer-api-zh_CN/#/)
## 我的
[dy-music-spider](https://github.com/Kylin93CN/dy-music-spider)

# 爬取过程
## 1.分析页面
`http://www.douyingequ.com/` 首页下有几个子页面，每个页面下的歌曲列表分为f1，f2，f3 三个id的div下的有序列表：如图
{% asset_img index.png 首页 %}
{% asset_img fenxi1.png 子页面 %}

点击每个歌曲链接后跳转一个新的页面，找到播放器的`audio`，拿到`src`即可。
{% asset_img fenxi2.png 子页面 %}

## 2.代码解析
```javascript
// 启动puppeteer，返回一个浏览器
const browser = await puppeteer.launch();
// 通过浏览器打开一个页面
const page = await browser.newPage();
// 获取一个随机的ua，防止被服务器禁止访问，一般使用随机ua配合动态ip
let ua = ramdomUA.getRandom();
await page.setViewport({ width: 1280, height: 800 });
await page.setUserAgent(ua);
// 页面跳转到指定的网站
await page.goto('http://www.douyingequ.com/douyinshenqu.htm ');
```

```javascript
// 使用page.$$eval 获取每个li下的跳转地址
let musicUrlList = await page.$$eval('#f1 li>a', eles => eles.map(ele => ele.href));
const musicUrlList2 = await page.$$eval('#f2 li>a', eles => eles.map(ele => ele.href));
const musicUrlList3 = await page.$$eval('#f3 li>a', eles => eles.map(ele => ele.href));
// 需求完成，关闭页面
await page.close();
```

```javascript
for (let index = 48; index < musicUrlList.length; index += 1) {
  const musicUr = musicUrlList[index];
  // 新开一个页面
  const musicPage = await browser.newPage();
  ua = ramdomUA.getRandom();
  await musicPage.setUserAgent(ua);
  // 跳转到某个音乐播放的页面
  await musicPage.goto(musicUr);
  // 获取音乐的信息
  let songName = await musicPage.$eval('.playingTit h1', el => el.innerText);
  let singer = await musicPage.$eval('.playingTit h2', el => el.innerText);
  const url = await musicPage.$eval('#kuPlayer audio', el => el.src);
  const ext = path.extname(url);
  // 处理歌名中特殊符号
  songName = songName.replace(/\//g, '&');
  singer = singer.replace(/\//g, '&');
  console.log(`歌手：${singer}&&歌名：${songName}&&下载地址：${url}`)
  
  await musicPage.close();
  // 下载
  if (!url) continue;
  downloadFile(url, path.resolve(__dirname, 'music/', `${songName}-${singer}${ext}`), () => {
    console.log(`第${index + 1}首歌曲----${songName}-----下载完毕！剩余${musicUrlList.length - index - 1}首`);
  });
}
```

## 3.代码执行
```shell
node src/index.js
```