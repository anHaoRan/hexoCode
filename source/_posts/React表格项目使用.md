---
title: React表格项目使用
date: 2017-04-16 03:24:47
tags: table
toc: true
---
总结只有两个字“很坑”。
个人理解，用表格。要看'行'里看列，而不是从'列'里看行。
demo：

```
<!--more--> 
  <table>  

    <thead>  

      <tr>  

            <td rowSpan={2}>这是头部1</td>  

            <td rowSpan={2}>这是头部1</td>  

            <td rowSpan={2}>这是头部1</td>  

            <td rowSpan={2}>这是头部1</td>  

            <td rowSpan={2}>这是头部1</td>  

            <td rowSpan={2}>这是头部1</td>  

            <td rowSpan={2}>这是头部1</td>  

            <td rowSpan={2}>这是头部1</td>  

            <td colSpan={6}>这是头部1</td>  

            <td colSpan={7}>这是头部1</td>  

            <td colSpan={2}>这是头部1</td>  

            <td colSpan={3}>这是头部1</td>  

            <td rowSpan={2}>这是头部1</td>  

            <td rowSpan={2}>这是头部1</td>  

            <td rowSpan={2}>这是头部1</td>  

      </tr>  

      <tr>  

          <td></td>  

          <td></td>  

          <td></td>  

          <td></td>  

          <td></td>  

          <td></td>  

          <td></td>  

          <td></td>  

          <td></td>  

          <td></td>  

          <td></td>  

          <td></td>  

          <td></td>  

          <td></td>  

          <td></td>  

          <td></td>  

          <td></td>  

          <td></td>  

        </tr>  

    </thead>  

    <tbody>  

      <tr>  
        <td>12</td>  

        <td>1</td>  

        <td>2017-5</td>  

        <td>759849</td>  

        <td>21</td>  

        <td>21</td>  

        <td>1004340000</td>  

        <td>546</td>  

        <td>656</td>  

        <td>3</td>  

        <td>546他</td>  

        <td>1000</td>  

        <td>400</td>  

        <td>600</td>  

        <td>100</td>  

        <td>100</td>  

        <td>100</td>  

        <td>321</td>  

        <td>300</td>  

        <td>201</td>  

        <td>321</td>  

        <td>321</td>  

        <td>321</td>  

        <td>-100</td>  

        <td>-100</td>  

        <td>-100</td>  

        <td>/</td>  

        <td>10000</td>  

        <td>sdfgerefd</td>  

      </tr>  

    </tbody>  

  </table>  

  ``