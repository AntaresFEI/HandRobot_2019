# HandRobot_2019
Bachor Design with Dong and Zhai


第一部分：
Control with leapmotion

第二部分：
Train with self-obtained data (images of hands)

Implement Simple tf-based algorithm to classify basic hand gesture on Raspberry

Use a web-camera to detect which would person act, i.e. Rock Paper Scissors

Machine makes suitable response to beat the player.

第三部分：
本模型构建的过程基本原理是通过Convolutional Pose Machine (卷积姿势机) 实现关键点的追踪，通过31个高清摄像头，对于已知关键点的图像进行处理，从而计算出每一层卷积的权重比。在使用该模型时，是对摄像头捕捉的每一帧画面，遍历其每一个像素点，计算每一点对应某一关键点的概率，找到全图中对应某一关键点概率最大的点，并且只有当其概率值超过门槛值时，才可以认为这一点就是所对应的关键点。

因而模型输出是可能包含空元素的22个坐标构成的矩阵。在图中，使用Open CV 函数库对这22个点进行处理，并通过直线连接某些点，形成手势的骨架，从而实现手指的追踪。

在此基础上，由于模型输出了22个数对的坐标值，通过实时计算当前画面中对应手指指尖点与手掌点的距离，以及并且记录手掌平在放时此距离的基准值，也就是最大值。并且引入了距离比例的概念，通过关键点（0,9）之间的基准距离（最大值）以及目前的距离，计算得到距离比例。

然后在考虑距离比例的基础上，通过目前的距离与最大距离的比例，构建三角形模型，计算出手指与掌面的夹角，输出给舵机，使其复现了图中的手指的位移。但是由于本组电脑并没有独立显卡，不能通过GPU加速Caffe Model的运算，每一帧画面的处理时间超乎预期，未能实现实时地跟踪，但是通过输入视频的方式，可以验证该算法的可行性。
