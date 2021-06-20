[TOC]



StateMachine接口需要以下4种泛型参数

- T代表实现的状态机类型
- S代表实现的状态类型
- E代表实现的事件类型
- C代表实现的外部上下文类型





entry -> exit 



State A 转换 State B

entry "A" -> @OnTransitionBegin(带条件的) - > @OnTransitionBegin(不带条件的) ->  exit "A" -> entry "B" -> @OnBeforeActionExecuted -> callMethod -> @OnTransitionComplete -> @OnTransitionEnd



tranistionBegin -> exit -> transition -> entry -> transitionComplete -> transitionEnd



事件处理过程

- 状态正常转移

  TransitionBegin -- (exit -> transition -> entry) --> TransitionComplete -> TransitionEnd

- 状态迁移异常

  TransitionBegin -- (exit->transition->entry) --> TransitionException --> TransitionEnd

- 状态迁移事件拒绝

  TransitionBegin-->TransitionDeclined --> TransitionEnd



