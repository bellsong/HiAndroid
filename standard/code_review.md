# Code Review

## Code Review是什么

    提交代码前需要其他人过一遍。

## 为什么要做Code Review

    Code Review是一个相互学习促进的过程，团队开发里面最有效，质量保证方面性价比最高的手段之一。主要达到质量保证、技术积累和风险控制三个目的。

## 怎么做

1. 结对。一带一路的方式，熟悉业务的有经验的人员从前期需求评审、方案设计到具体编码过程都一定程度的参与，跟实际开发人员的讨论完善，最终达成一致方案并落实。

1. 工具可以用gerrit、gitlab、github等

2. 团队要求提交到主线或者发布分支的代码，必须经过一位以上熟悉业务、技术把控能力强、有codereview经验的人员进行code review

3. 改动的代码涉及到其他模块或者公用组件的，要指定专项的负责人或者模块负责人review

## 

2）Review规范

a. 流程：feature代码分支开发 -> 算法co-review -> 合并到release -> 架构review -> 合并到dev和master

b. 结对Review：UC团队的代码需要提交到B角Review，神马团队的代码需要提交到A角Review

c. 时间：审核owner必须在24小时内给反馈，否则视为没有问题

d. 责任：对于代码级别的线上问题，上线owner和审核owner一起承担

注：该规范先在leaf server上执行，后续其它服务有类似较多的协作，再做更新。


CodeReview的初衷是提升效率，特别是提高软件质量，避免在修Bug时引入新问题，尽量不给我们自己增加负担。

我建议开发过程中不强制要求，强制执行的有以下几个场景：
1、正式提测后
2、修复线上Bug
3、其他需要保证质量的场景；

总的方针是：在开发阶段侧重效率，提测阶段侧重质量。

