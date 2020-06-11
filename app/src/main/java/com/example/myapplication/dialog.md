## Dialog对话框及自定义 Dialog对话框
### 
/**
 * 种类：
 * 1.简单对话框
 * 2.进度对话框
 * 3.日期对话框
 * 4.时间对话框
 * 5.列表对话框
 * 6.单选对话框
 * 7.多选对话框
 * 8.自定义Dialog
 * 9.自定义日期对话框
 * 10.Dialog Activity
 * (将activity做为dialog来显示）
 *
 */
 
 ~~~
 public class MainActivity extends AppCompatActivity {


    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
    }

    //简单对话框
    private void showSimpleDialog(){
        AlertDialog.Builder builder = new AlertDialog.Builder(this);
        builder.setTitle("温馨提示");
        builder.setIcon(R.mipmap.bq1);
        builder.setMessage("确定要删除这条信息吗？");
        builder.setNegativeButton("取消", new DialogInterface.OnClickListener() {
            @Override
            public void onClick(DialogInterface dialog, int which) {
                Toast.makeText(MainActivity.this, "no", Toast.LENGTH_SHORT).show();
            }
        });
        builder.setPositiveButton("确定", new DialogInterface.OnClickListener() {
            @Override
            public void onClick(DialogInterface dialog, int which) {
                Toast.makeText(MainActivity.this, "yes", Toast.LENGTH_SHORT).show();
            }
        });
        AlertDialog dialog = builder.create();
        dialog.setCanceledOnTouchOutside(false);
        dialog.show();
    }

    /**
       进度条对话框
     * onStart()：不调用此方法则setProgress()不生效
     */
    private void showProgressDialog(){

        final ProgressDialog progressDialog = new ProgressDialog(this);
        progressDialog.setTitle("loading....");
        progressDialog.setIcon(R.mipmap.bq1);
        progressDialog.setProgressStyle(ProgressDialog.STYLE_HORIZONTAL);
        progressDialog.setMax(100);
        progressDialog.onStart();
        progressDialog.show();

        new Thread(){
            @Override
            public void run() {
                super.run();
                try {
                    for (int i = 1;i<=100;i++){
                        Thread.sleep(200);
                       progressDialog.setProgress(i);
                       if(progressDialog.getProgress() == 100){
                           progressDialog.cancel();
                       }
                    }
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        }.start();
    }

    //日期对话框
    private void showDateDialog(){
        Calendar calendar = Calendar.getInstance();
        calendar.set(2020,5,10);
        DatePickerDialog datePickerDialog = new DatePickerDialog(this, DatePickerDialog.BUTTON_NEGATIVE,
                new DatePickerDialog.OnDateSetListener() {
                    @Override
                    public void onDateSet(DatePicker view, int year, int month, int dayOfMonth) {
                        Toast.makeText(MainActivity.this, "年"+year+"月"+month+"日"+dayOfMonth, Toast.LENGTH_SHORT).show();
                    }
                }, 2020, 5, 10);
        datePickerDialog.show();
    }

    //时间对话框
    private void showTimeDialog(){
        TimePickerDialog timePickerDialog = new TimePickerDialog(this, new TimePickerDialog.OnTimeSetListener() {
            @Override
            public void onTimeSet(TimePicker view, int hourOfDay, int minute) {
                Toast.makeText(MainActivity.this, "现在时间："+hourOfDay+"："+minute+"", Toast.LENGTH_SHORT).show();
            }
        },17,48,true);
        timePickerDialog.show();
    }

    //列表、单选、多选对话框
    private void showItemDialog(){
        AlertDialog.Builder builder = new AlertDialog.Builder(this);
        builder.setTitle("请选择以下喜欢的水果");
        builder.setIcon(R.mipmap.bq1);
        final String[] items = new String[]{"苹果", "香蕉", "葡萄"};

        //单选对话框
//        builder.setSingleChoiceItems(items, 1, new DialogInterface.OnClickListener() {
//            @Override
//            public void onClick(DialogInterface dialog, int which) {
//                Toast.makeText(MainActivity.this, "您选择的下标是："+which+"-"+items[which], Toast.LENGTH_SHORT).show();
//            }
//        });

        //列表对话框
//        builder.setItems(items, new DialogInterface.OnClickListener() {
//            @Override
//            public void onClick(DialogInterface dialog, int which) {
//                Toast.makeText(MainActivity.this, "您选择的下标是："+which+"-"+items[which], Toast.LENGTH_SHORT).show();
//            }
//        });

        //多选对话框
        builder.setMultiChoiceItems(items, new boolean[]{true,true,false}, new DialogInterface.OnMultiChoiceClickListener() {
            @Override
            public void onClick(DialogInterface dialog, int which, boolean isChecked) {
                if(isChecked == false){
                    Toast.makeText(MainActivity.this, "您不喜欢："+which+"-"+items[which], Toast.LENGTH_SHORT).show();
                }
                else{
                    Toast.makeText(MainActivity.this, "您喜欢："+which+"-"+items[which], Toast.LENGTH_SHORT).show();
                }
            }
        });

        AlertDialog dialog = builder.create();
        dialog.show();
    }

    //自定义对话框
    private void showCumtomDialog(){
        final AlertDialog.Builder builder = new AlertDialog.Builder(this);
        View view = LayoutInflater.from(getApplicationContext()).inflate(R.layout.custom_dialog,null);
        builder.setView(view);
        TextView yes = view.findViewById(R.id.txt_yes);
        final AlertDialog dialog = builder.create();
        dialog.show();
        yes.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Toast.makeText(MainActivity.this, "点了确定取消", Toast.LENGTH_SHORT).show();
                dialog.dismiss();
            }
        });
    }

    public void clickAlertDialog(View view) {
        showSimpleDialog();
    }

    public void clickProgressDialog(View view) {
        showProgressDialog();
    }

    public void clickDateDialog(View view) {
        showDateDialog();
    }

    public void clickTimeDialog(View view) {
        showTimeDialog();
    }

    public void clickItemDialog(View view) {
        showItemDialog();
    }

    public void clickCustomDialog(View view) {
        showCumtomDialog();
    }


}
 ~~~
 
### 一.dialogActivity
#### 步骤：
####    1.编写style.xml
####        2.修改AndroidManifest.xml文件中activity主题
    
 ~~~
 <?xml version="1.0" encoding="utf-8"?>
<resources>
    <style name="dialog_style" parent="@style/AppTheme">
        <!--是否悬浮在activity上-->
        <item name="android:windowIsFloating">true</item>
        <!--是否透明-->
        <item name="android:windowIsTranslucent">true</item>
        <item name="android:title">"activity dialog"</item>
        <item name="android:dialogTitle">"对话框标题"</item>
    </style>
</resources>
 ~~~
 ~~~
    <activity android:name=".MainActivity" android:theme="@style/dialog_style">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
 ~~~