+++
date = '2024-11-14T18:59:45+08:00'
draft = true
title = 'Phaser开发简单的2d小游戏demo'
+++

### 1.安装

使用如下的命令之一就可以获取工程费的phaser项目。

```bash
npm create @phaserjs/game@latest
npx @phaserjs/create-game@latest
yarn create @phaserjs/game
pnpm create @phaserjs/game@latest
bun create @phaserjs/game@latest
```

或者使用

```bash
npm install phaser
```

安装npm包在其他项目使用。

也可以使用

```html
<script src="//cdn.jsdelivr.net/npm/phaser@3.86.0/dist/phaser.js"></script>
<script src="//cdn.jsdelivr.net/npm/phaser@3.86.0/dist/phaser.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/phaser/3.86.0/phaser.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/phaser/3.86.0/phaser.min.js"></script>
```

这些方式从cdn获取源码。

或者，按照本文的方式，从github直接下载源码在本地使用。

前往其[github仓库](https://github.com/phaserjs/phaser)下载dist目录下的phaser.js然后导入自己的项目中。

例如

```html
    <script src="js/phaser.js"></script>
```

### 2.预加载资源

接下来以本人模仿的玩具项目[google-dinosaur](https://github.com/ColaBlack/google-dinosaur)为例介绍如何使用phaser.js制作简单的小游戏。 

首先需要先创建一个游戏场景类，在构造函数中声明场景的名称。

```javascript
export default class PreLoad extends Phaser.Scene {
  constructor() {
    super("preLoadScene");
  }
}
```

当进入场景时phaser.js会先调用场景的preload方法。

在preload方法中调用以下两个函数就可以预加载音乐和图片资源。

```javascript
this.load.audio("音乐名称", "音乐地址");
this.load.image("图片名称", "图片地址");
```

而对于动画，我们只需要提供几个关键帧的图片，phaser.js会自动绘制动画，这种我们称之为精灵表单对象。使用这样的方法载入：

```javascript
this.load.spritesheet(
    "关键帧的名称",
    "关键帧的地址",
    {关键帧信息，如关键帧的大小等}
);
```

接下来phaser.js会调用create方法创建场景，在google-dinosaur项目中，create方法用于跳转到gameScene场景。

```javascript
this.scene.start("gameScene");
```

最后，给一下第一个场景的完整代码。

```javascript
/**
 * 在游戏开始前预加载资源
 */
export default class PreLoad extends Phaser.Scene {
  constructor() {
    super("preLoadScene");
  }

  preload() {
    // 加载背景音乐

    // 死亡音效
    this.load.audio("dead", "./assets/audio/Dead.wav");
    // 跳跃音效
    this.load.audio("jump", "./assets/audio/Jump.wav");
    // 得分音效
    this.load.audio("score", "./assets/audio/Score.wav");

    // 加载图片资源

    for (let i = 1; i <= 5; i++) {
      // 5张仙人掌图片
      this.load.image(`cactus-${i}`, `./assets/images/Cactus-${i}.png`);
    }

    // 地面图片
    this.load.image("ground", "./assets/images/Ground.png");

    // 游戏结束文字图片
    this.load.image("gameover_text", "./assets/images/Gameover_text.png");

    // 再玩一次按钮图片
    this.load.image("replay_button", "./assets/images/Replay_button.png");

    for (let i = 1; i <= 4; i++) {
      // 4张小恐龙图片
      this.load.spritesheet(
        `dinosaur-${i}`,
        `./assets/images/Dinosaur-${i}.png`,
        { frameWidth: 88, frameHeight: 94 }
      );
    }
  }

  create() {
    // 切换到游戏场景
    this.scene.start("gameScene");
  }
}

```

### 2.创建场景

在介绍如何创建场景前，我们先写gameScene场景的代码框架，也就是

```javascript
export default class GameScene extends Phaser.Scene {
  constructor() {
    super("gameScene");
  }
}
```

由于资源已经在前面预加载完了所以这里不需要预加载资源，我们直接写create方法即可。

###### 2.1创建游戏对象

在创建场景的时候主要的代码是创建游戏需要的对象，以google-dinosaur项目为例，需要创建地面、小恐龙、仙人掌等对象。

地面的宽度和游戏画布的宽度一样，高度则位与中间，这意味着我们要先获取画布的高度和宽度。

```javascript
    // 获取游戏画布的宽度和高度
    this.width = this.sys.game.config.width;
    this.height = this.sys.game.config.height;
```

地面是静态不动的，对于这种静态的对象要用`this.physics.add.staticGroup()`方法先创建静态对象组。

然后创建对象

```javascript
this.ground
  .create(this.width / 2, this.height - 13, "ground")
  .setScale(2)
  .refreshBody();
```

create方法的前两个参数是对象的坐标，在phaser.js中默认的坐标系是从左上角开始,越往右x越大，越往下y越大。（当然，这个是可以自己改的）而这里的坐标默认是以对象的左上角为标准的。(左上角也被称为原点，也是可以自己改动的)

例如你想把一个图片的左上角放到(0,0)，那就直接在create里输入0,0就可以了。

"ground"是刚刚预加载的图片对象的名字，有这个参数phaser.js才知道刚刚创建的地面对象的贴图是ground。

` setScale(2)`则是将地面放大了2倍,`refreshBody()`要求phaser.js重新计算刷新地面对象的物理属性。

对于会运动且有物理碰撞效果的一般的对象，可以用`this.physics.add.group()`为他们创造组。

针对精灵表单和其他的一般对象可以用这样的方式创建

```javascript
    this.dinosaur = this.physics.add
      .sprite(50, this.height / 2, "dinosaur-1")
      .setOrigin(0, 1);
```

 ` dinosaur-1`是动画的第一帧或者是对象的贴图。`setOrigin`则将该对象的原点设置到了x为0%，y为100%也就是对象的左下角了。

如果要加入碰撞效果呢，就可以用这样的方法

```javascript
// 让小恐龙与地面发生碰撞，防止小恐龙掉落
this.physics.add.collider(this.dinosaur, this.ground);
```

###### 2.2初始化动画

参考google-dinosaur的代码，

```javascript
    // 定义恐龙奔跑的动画
    this.dinosaur.anims.create({
      key: "run",
      frames: [
        { key: "dinosaur-1" },
        { key: "dinosaur-2" },
        { key: "dinosaur-3" },
        { key: "dinosaur-4" },
      ],
      frameRate: 10,
      repeat: -1,
    });
```

精灵表单的`anims.create`方法可以初始化动画，`key`则给了动画一个名字，叫做run。`frames`数组则列举了动画需要的关键帧的名字。`frameRate`是帧速率，`repeat`则说明了动画需要重复几次，-1表示动画要一直重复。

###### 初始化音乐

初始化音乐就更简单了。使用`this.sound.add("音乐名称")`就可以了。

以google-dinosaur为例只要这样就能初始化三个音乐了。

```javascript
    this.deadSound = this.sound.add("dead");
    this.jumpSound = this.sound.add("jump");
    this.scoreSound = this.sound.add("score");
```

###### 监听用户输入

这里以监听用户键盘输入为例，使用

```javascript
this.input.keyboard.on("keydown", this.handleJump.bind(this), this);
```

`keydown`表示监听用户的任意键盘输入，也就是用户按任意键就调用`handleJump`方法。

`handleJump`方法的代码很简单，就不说了。

```javascript
  handleJump() {
    // 如果小恐龙不在地面上，则不执行跳跃以避免用户进行二段跳
    if (!this.dinosaur.body.onFloor()) {
      return;
    }

    // 跳跃，设置y的速度为-1500像素每秒(负速度的方向向上)
    this.dinosaur.setVelocityY(-1500);
    // 播放跳跃音效
    this.jumpSound.play();
  }
```

### 3.编写每一帧的处理

接下来phaser.js会在游戏的每一帧都调用update函数

这个部分只有播放精灵表单的动画是通用的。

```javascript
    // 播放奔跑动画
    this.dinosaur.anims.play("run", true);
```

这个语句就可以播放刚刚初始化的小恐龙奔跑动画。

其他的语句都不是通用的，我就直接放google-dinosaur的源码给各位参考了。

```javascript
  update(time, delta) {

    // 播放奔跑动画
    this.dinosaur.anims.play("run", true);

    // 检测小恐龙是否跳过仙人掌
    this.cactusGroup.getChildren().forEach((cactus) => {
      // 如果仙人掌的x坐标小于50且未被计分
      if (cactus.x < 50 && !cactus.getData("scored")) {
        // 标记仙人掌为已计分，避免重复计分
        cactus.setData("scored", true);
        // 播放得分音效
        this.scoreSound.play();
      }
    });

    // 定义计时器，每隔1-5秒生成一个仙人掌
    this.timer = this.timer || time;

    const cactusInterval = Phaser.Math.Clamp(
      2000 - this.speed * 100,
      500,
      2000
    ); // 最小间隔为500ms，最大间隔为2000ms

    if (time - this.timer > cactusInterval) {
      this.summonCactus();
      this.timer = time;
    }
  }
```

### 4.定义对象交互逻辑

接下来需要游戏其实就已经开始进行了，但是我们还没有定义对象之间的关系和结束游戏的方法。

例如，在google-dinosaur项目中，当小恐龙和仙人掌碰撞的时候应该停止游戏。

不过这个你可能得回去修改刚刚写的代码，例如修改增加小恐龙和仙人掌碰撞的代码为

```javascript
    // 添加小恐龙与仙人掌组之间的碰撞检测
    this.physics.add.collider(
      this.dinosaur,
      this.cactusGroup,
      this.handleDinoCactusCollision,
      null,
      this
    );
```

这个代码既增加了二者的碰撞，又定义了当二者碰撞的时候调用`handleDinoCactusCollision`函数。

这部分其实也没有通用的代码。

大致上也就这三个代码比较常用

```javascript
    // 播放死亡音效
    this.deadSound.play();
    
    // 停止物理模拟
    this.physics.pause();
      
    // 停止所有动画
    this.anims.pauseAll();
```

为了处理google-dinosaur的特殊需求，在该项目里这个函数其实是这样的。

```javascript
  handleDinoCactusCollision(dinosaur, cactus) {
    // 播放死亡音效
    this.deadSound.play();
    
    // 停止物理模拟
    this.physics.pause();
      
    // 停止所有动画
    this.anims.pauseAll();
      
    
    this.speed = 1;
    this.score = 0;
    // 游戏结束时停止得分增加
    if (this.scoreEvent) {
      this.scoreEvent.remove(false);
    }
    this.handleGameOver();
  }

  handleGameOver() {
    // 显示游戏结束文字
    const gameOverText = this.add
      .image(this.width / 2, this.height / 2 - 100, "gameover_text")
      .setScale(2);

    // 显示再玩一次按钮
    const replayButton = this.add
      .image(this.width / 2, this.height / 2 + 100, "replay_button")
      .setInteractive()
      .setScale(2);

    // 为再玩一次按钮添加点击事件监听器
    replayButton.on("pointerdown", () => {
      this.scene.restart(); // 重新开始当前场景
    });
  }
```

### 5.配置并启动游戏

通过前面的步骤你已经完成了绝大部分代码。接下来只需要配置一下然后就可以启动游戏啦。

前面的代码就只是定义了两个类，除了一堆定义之外其实没有代码被执行。

如果想真正地启动游戏，只需要一句话：

```javascript
// 创建游戏实例
const game = new Phaser.Game(config);
```

config是游戏的配置对象，这里以google-dinosaur项目的配置为例。

```javascript
// 游戏配置
const config = {
  type: Phaser.AUTO, // 游戏渲染器类型
  width: 800,// 游戏画布宽度
  height: 300,// 游戏画布高度
  backgroundColor: "#ffffff", //修改背景色为白色
  parent: "game", // 游戏绑定的父标签的id
  scene: [PreLoad, GameScene], // 游戏场景列表，会先进入第一个场景
  physics: {
    default: "arcade", // 默认物理引擎
    arcade: {
      gravity: { y: 5000 }, // 重力设置
      debug: false, // 调试模式，开发时使用
    },
  },
};
```

每一个配置项我都写了注释，应该可以直接看懂了。

在这里，游戏渲染器类型你可能不知道是什么意思，其实就是调整游戏是用webGL渲染还是canvas渲染。一般选自动就可以了。
