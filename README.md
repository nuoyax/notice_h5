# notice_h5

.announcement-box 是外层盒子，它设置了 height: 50px 并且使用 overflow: hidden 来隐藏超出内容区域的部分。
.announcements 是包含所有公告的容器，它通过 position: absolute 设置位置，并通过 @keyframes scroll 动画来实现上下滚动。
每条公告的 height 都是 50px，对应公告盒子的高度。
动画 scroll 通过关键帧控制在每个公告停留2秒（例如 16% 和 36% 的设置），然后滚动到下一条公告。

上下公告轮播实现

为了使最后一条公告和第一条公告之间的过渡更加平滑，消除跳跃感，我们可以修改动画的方式，使其在最后一条公告滚动结束时无缝地连接到第一条公告。实现的关键是添加一个虚拟的副本，将第一个公告再次显示在最后，这样动画可以无缝循环。

公告的停留时间是在 @keyframes 动画的百分比进度中计算出来的。我们首先需要理解动画的 总时长，然后根据每条公告的停留时间和过渡时间来设置关键帧。

在你的例子中，假设动画总时长为 12s，这包括所有公告滚动和每条公告的停留时间。我们需要为每个阶段（公告的停留时间和滚动到下一条的时间）分配合适的百分比。

计算步骤：
每条公告的停留时间：我们设定每条公告停留 2秒，这是一个固定的时间。

滚动过渡时间：每次滚动过渡的时间也需要分配一定的时间。为了过渡看起来流畅，我们可以给过渡过程分配一个较短的时间，比如 0.5秒。

计算总的百分比：CSS 动画使用的是百分比（0% 到 100%），所以我们需要把这些时间换算成动画的进度百分比。

计算方式：
总动画时长 = 12秒
每条公告停留时间 = 2秒
每次滚动过渡时间 = 0.5秒
每个滚动周期由停留时间和过渡时间组成：

周期时间 = 2秒（停留） + 0.5秒（滚动） = 2.5秒
5段公告 + 无缝副本的第1条公告：总共是6段滚动。

百分比计算：
每个完整周期占动画总时间的百分比 = 2.5秒 / 12秒 ≈ 20.83%
停留时间占总时间的百分比 = 2秒 / 12秒 ≈ 16.67%
滚动时间占总时间的百分比 = 0.5秒 / 12秒 ≈ 4.17%
应用在关键帧中的比例：
我们可以通过关键帧动画控制每个公告的停留和滚动位置：

公告 1：停留 0% 到 16.67%，过渡 16.67% 到 20.83%
公告 2：停留 20.83% 到 37.5%，过渡 37.5% 到 41.67%
公告 3：停留 41.67% 到 58.33%，过渡 58.33% 到 62.5%
公告 4：停留 62.5% 到 79.17%，过渡 79.17% 到 83.33%
公告 5：停留 83.33% 到 100%，然后从副本滚回第一条。
根据这个计算结果，改进后的关键帧动画如下：

css
复制代码
@keyframes scroll {
    0% { top: 0; }
    16.67% { top: 0; } /*第一条公告停留 */
    20.83% { top: -50px; } /* 滚动到第二条*/

    37.5% { top: -50px; } /* 第二条公告停留 */
    41.67% { top: -100px; } /* 滚动到第三条 */
    
    58.33% { top: -100px; } /* 第三条公告停留 */
    62.5% { top: -150px; } /* 滚动到第四条 */
    
    79.17% { top: -150px; } /* 第四条公告停留 */
    83.33% { top: -200px; } /* 滚动到第五条 */
    
    100% { top: -250px; } /* 第五条公告停留，滚动到副本的第一条，实现无缝 */
}
解释：
每条公告有 16.67% 的时间在当前的位置停留。
每次滚动有 4.17% 的时间用于过渡到下一条公告。
这样滚动的每个周期保持一致，并在滚动到第一个公告副本时无缝连接到原始的第一条公告，确保没有跳跃感。
动画时长的灵活性：
如果你想调整停留或滚动的速度，可以改变 总时长 或 过渡时间，相应调整百分比比例即可。
