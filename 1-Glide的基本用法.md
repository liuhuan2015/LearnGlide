>android上的图片加载框架目前非常成熟，有最早的UIL，Google的Volley，再后来的Glide和Picasso，Facebook的Fresco，都非常稳定，功能也非常强大。<br>
Glide是Google官方推荐的图片加载框架，使用起来比较容易；Picasso是著名的square公司发布的，代码贡献成员中有Jake Wharton；Fresco是Facebook开源出来的图片加载框架，功能上比较强大。

#### 1. 加载图片

```java
     Glide.with(this).load(url).into(imageView);
```

##### 1.1 with(this) 方法

加载图片只需要这一行代码就可以了，需要注意的是 with(this) 方法中的参数，它可以是Context，Activity，或者Fragment类型的参数，即选择的范围非常广，<br>
如果调用的地方既不在Activity中也不在Fragment中，也没有关系，可以获取当前应用程序的ApplicationContext，传入到 with() 方法中，<br>
传入的这个上下文会决定Glide加载图片的生命周期，如果传入的是Activity或者Fragment的实例，那么当这个Activity或Fragment被销毁的时候，图片加载也会停止。<br>
如果传入的是ApplicationContext，那么只有当应用程序被杀掉的时候，图片加载才会停止。

##### 1.2 load(url) 方法

这个方法用于指定待加载的图片资源。Glide支持加载各种各样的图片资源，包括网络图片、本地图片、应用资源、二进制流、Uri对象等等

##### 1.3 into(imageView) 方法

这个方法表示我们要让图片显示在那个ImageView上。into() 方法不仅仅是只能接收ImageView类型的参数，它还支持很多的用法，这个属于高级技巧。

#### 2. 占位图

```java
        Glide.with(this)
                .load(url)
                .placeholder(R.drawable.ic_launcher_foreground)
                //.diskCacheStrategy(DiskCacheStrategy.NONE)
                .error(R.drawable.error)
                .into(imageView);
```
placeholder()：用于指定图片加载过程时的占位符；Glide有非常强大的缓存机制，图片首次加载后会被Glide缓存下来，下次加载的时候会直接从缓存中读取，不会再去网络下载。<br>

diskCacheStrategy() 指定缓存策略；diskCacheStrategy(DiskCacheStrategy.NONE)：禁用缓存；缓存方面的内容将在后面详细讲解。

error(R.drawable.error)：图片加载失败时的占位符（因为某些情况导致图片加载失败，比如网络信号不好）

#### 3. 指定图片格式

Glide支持加载 Gif 图片

把前面的图片url换成一张gif图片的，
```java
    http://p1.pstatp.com/large/166200019850062839d3
```
然后重新运行代码，就可以看到效果了。

即：不管传入的是一张普通图片，还是一张gif图片，Glide都会自动进行判断，并且可以正确的把它解析和展示出来。

Glide还可以手动指定图片格式：asBitmap()、asGif()

如果图片本身是一张gif，设置asBitmap()，会加载第一帧图片
如果图片本身是一张静态图，设置asGif()，会加载失败

```java
        Glide.with(this)
                .load(url)
                .asBitmap() //指定图片格式为一张静态图片，即只允许加载静态图片
                .placeholder(R.drawable.ic_launcher_foreground)
                .diskCacheStrategy(DiskCacheStrategy.NONE)
                .error(R.drawable.error)
                .into(imageView);
```
#### 4. 指定图片大小

在使用Glide的绝大多数情况下我们是不需要指定图片大小的。

如果我们要加载一张1000*1000像素的图片，但是在界面上的ImageView可能只有200*200像素，此时如果不对图片进行任何的压缩就直接读取到内存中，这就属于内存浪费了，
因为程序中根本就用不到这么高像素的图片。

使用Glide的话，不需要担心这个，因为Glide会自动判断ImageView的大小，然后只将这么大的图片像素加载到内存当中，帮助我们节省内存开支。

原理[https://blog.csdn.net/guolin_blog/article/details/9316683](Android高效加载大图、多图解决方案，有效避免程序OOM)

如果真的有这个需求，可以使用 override() 方法
```java
    Glide.with(this)
         .load(url)
         .placeholder(R.drawable.loading)
         .error(R.drawable.error)
         .diskCacheStrategy(DiskCacheStrategy.NONE)
         .override(100, 100)
         .into(imageView);
```









