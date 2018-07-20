---
layout: post
title: "React/React Native + Heatmap"
author: "Zhengyu Wu"
---


How to combine React/React Native and the existing heatmap demo?

## Awesome JavaScript Libraries for plotting charts

There are many free JavaScript Libraries for plotting charts in web pages, such as [D3js](https://d3js.org/), [HIGHCHART](https://www.highcharts.com/) and [EChart](https://www.highcharts.com/). 

It's very handy to use the APIs of these tools. You can even just download the source code of demos and change the input data.

Among these charts, the heatmap is applied in certain applications for some specific purposes. Almost every above JavaScript library provodes the heatmap demo. There is even a lightweight Javascript library, [heatmap.js](https://www.patrick-wied.at/static/heatmapjs/), which only concentrates on helping you visualize your heatmap. And luckily, this tool is free for students.


However, it is not simple to embed these demos in famous frontend frameworks, such as React or React Native. It means you have to turn the heatmap demo to a React component. Things come difficult now. First, you need to read the code of the heatmap demo and understand the function of each module. Second, you are supposed to rewrite the code to match the style of a React component. Last but not least, the modified heatmap should blend in your original web page or Android page. 

Now our team is developing a project called Smart Garden which includes a Web website and an Android App. The project requires a heatmap to show the temperature distribution of a garden.

## React + heatmap

I tried many approaches but failed. Finally, I found a powerful library which is called [Bizcharts](https://alibaba.github.io/BizCharts/) supported by Alibaba.

Bizcharts can perfectly coordinate with React framework which means you don't need to bother modifying the code of the demo.

You only need to follow the steps as follows.

**STEP 1**: Type in the following command in your bash or terminal.

```
npm install bizchart
``` 

**STEP 2**: Downlaod the demo and add the sentence in the App.js of your React dictinary.

```
import { Chart, Geom, Tooltip, Legend,Guide } from 'bizcharts';
```

**STEP 3**: Replace your own data with the test data.

<br>

And then, you'll definitely get the following heatmap figure.


![](https://github.com/GEORGE5961/Smart-Garden/raw/master/code/heatmap_component/pic.png?raw=true)

## React Native + heatmap

It's also a long journey to find the suitable dependence to draw the heatmap.

I tried Google map, but it needs a API key and the tutorial on the Internet didn't fit my directory structure. So as the Baidu map.

At last, I turn to my old friend, Echarts, which has the same syntax rules in both React and React Native.

Its usage is also babysitting.

All you need do is to add the following sentence in your js file.

```
import Echarts from 'native-echarts';
```
Then you just untilize the demo and replace with your real input.

For your convience, all the codes are presented on Github [https://github.com/GEORGE5961/Smart-Garden/](https://github.com/GEORGE5961/Smart-Garden/).




