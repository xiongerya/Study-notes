# MVC和MVVM
## MVC
>MVC(Model-View-Controller)，是一种常见的软件构架模式，所有的通信都是单向的。
>分为两种通信模式，View接收指令，传递给controller；或者直接controller接收指令。

- Model：数据层（核心），存储程序需要操作的数据/信息，在view上展示给用户
- View：视图层，提供给用户的操纵界面，用户进行操纵并传送指令到controller
- Controller：控制层，根据接收指令，对model进行业务逻辑操作

## MVP
>MVP(Model-View-Presenter)，改变了通信状态，各部分可以进行双向通信，但是View和Model之间不发生联系。由Presenter负责数据和视图之间的双向联系。

## MVVM
>MVVM(Model-View-ViewModel)，与MVP类似，View和Model之间不发生联系。VM与View之间是双向绑定，V的变化会反应在VM中。

- Model：数据层，业务逻辑处理和数据库
- View：视图层，与用户交互交互的界面，与VM进行双向数据绑定
- ViewModel：视图数据模型，可与Model进行Ajax和JSON等数据请求