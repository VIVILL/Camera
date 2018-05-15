
首先，android中使用camera需要在manifest添加权限（android6.0以上还需在代码中动态添加权限）。

    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
    <uses-permission android:name="android.permission.CAMERA"/>

主要代码如下：

    public class MainActivity extends AppCompatActivity  implements SurfaceHolder.Callback {
    private SurfaceView mSurfaceView;
    private Camera mCamera;
    private SurfaceHolder mSurfaceHolder;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        mSurfaceView=(SurfaceView) findViewById(R.id.surfaceView);
        mSurfaceHolder = mSurfaceView.getHolder();
        mSurfaceHolder.addCallback(this);// 回调函数接口surfaceCreated

          //camera相关可以放在surfaceCreated中也可以放在这个位置，但是mCamera.setPreviewDisplay(）必须要放在surfaceCreated中才会正常显示摄像机预览，为什么？
       
        mCamera = Camera.open(1);//有0和1两个摄像头，这里我打开的是1
        mCamera.startPreview();//

        }

    @Override
    public void surfaceCreated(SurfaceHolder surfaceHolder) {
        try {
            mCamera.setPreviewDisplay(surfaceHolder);//通过surfaceview显示取景画面
      //   mCamera.setPreviewDisplay(mSurfaceHolder);//这句也可以正常实现相机预览，但是也是必须放这里才可以，放上面不行
            } catch (IOException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
            }

        }

    @Override
    public void surfaceChanged(SurfaceHolder surfaceHolder, int i, int i1, int i2) {
  
        }

    @Override
    public void surfaceDestroyed(SurfaceHolder surfaceHolder) {

        }


    }


xml相关代码

    <?xml version="1.0" encoding="utf-8"?>
    <LinearLayout
        xmlns:android="http://schemas.android.com/apk/res/android"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:orientation="vertical">
        <SurfaceView
            android:id="@+id/surfaceView"
            android:layout_width="400dp"
            android:layout_height="300dp"
            android:layout_marginTop="30dp"/>
    </LinearLayout>





结果：

![](https://i.imgur.com/cOlnxF9.png)



以上就是最简单的一个利用surfaceview实现最简单的camera预览功能。

android  6.0权限申请可以参考这个
https://blog.csdn.net/thekingofcoding/article/details/55806164

