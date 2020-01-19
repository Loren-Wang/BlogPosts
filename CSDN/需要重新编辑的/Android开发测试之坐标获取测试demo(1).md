    安卓开发测试创建新的坐标获取demo，其中包含本机定位以及百度定位的测试，其中本机gps返回的是原始的gps坐标，其余的是直接返回正常坐标，主要类的代码：

```
package com.gpslocationtest.later;

import android.Manifest;
import android.content.Context;
import android.content.pm.PackageManager;
import android.location.Location;
import android.location.LocationListener;
import android.location.LocationManager;
import android.os.Bundle;
import android.os.Environment;
import android.os.Handler;
import android.os.Message;
import android.support.v4.app.ActivityCompat;
import android.support.v7.app.AppCompatActivity;
import android.util.Log;
import android.widget.ListView;

import com.baidu.location.BDLocation;
import com.baidu.location.LocationClientOption;
import com.gpslocationtest.R;

import java.io.BufferedWriter;
import java.io.File;
import java.io.FileWriter;
import java.text.SimpleDateFormat;
import java.util.ArrayList;
import java.util.Date;
import java.util.List;

/**
 * 创建人：王亮（Loren wang）
 * 创建时间：2017.3.9
 * 功能：测试主类
 * 
 */
public class LocationTestActivity extends AppCompatActivity {

    private ListView lvList1;
    private ListView lvList2;
    private Context context;
    private LocationManager locationManager;
    private int locationTimeSmallInterval = 1000;
    private int locationTimeSum = 5000;//总的定位时间
    private int locationTimeBigInterval = 15000;//大的时间间隔
    private Location bestLocation;

    private static final String LOCATION_GPS = "gps";
    private static final String LOCATION_NETWORK = "network";
    private static final String LOCATION_PASSIVE = "passive";
    private String savePhoneLOcationPath = Environment.getExternalStorageDirectory().getPath() + "/GpsLocationTest/" + "phoneLocationTest.txt";
    private String saveBaiduLocationPath = Environment.getExternalStorageDirectory().getPath() + "/GpsLocationTest/" + "baiduLocationTest.txt";

    private List<String> phoneLocationList = new ArrayList<>();
    private PhoneLocationAdapter phoneLocationAdapter;
    private List<String> baiduLocationList = new ArrayList<>();
    private BaiduLocationAdapter baiduLocationAdapter;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_location_test);
        context = this;
        lvList1 = (ListView) findViewById(R.id.lvList1);
        lvList2 = (ListView) findViewById(R.id.lvList2);

        initPhoneLocation();

        initBaiduLocation();
    }

    private LocationListener locationGpsListener;
    private LocationListener locationNetworkListener;
    private static final int TWO_MINUTES = 1000 * 60 * 2;

    private void initPhoneLocation() {
        phoneLocationAdapter = new PhoneLocationAdapter(context,phoneLocationList);
        lvList1.setAdapter(phoneLocationAdapter);

        initPhoneLocationListener();

        final int stop = 0;
        final int start = 1;
        final Handler handler = new Handler() {
            @Override
            public void handleMessage(Message msg) {
                super.handleMessage(msg);
                switch (msg.what) {
                    case stop:
                        if (locationManager != null) {
                            if (ActivityCompat.checkSelfPermission(context, Manifest.permission.ACCESS_FINE_LOCATION) != PackageManager.PERMISSION_GRANTED
                                    && ActivityCompat.checkSelfPermission(context, Manifest.permission.ACCESS_COARSE_LOCATION) != PackageManager.PERMISSION_GRANTED) {
                                // TODO: Consider calling
                                //    ActivityCompat#requestPermissions
                                // here to request the missing permissions, and then overriding
                                //   public void onRequestPermissionsResult(int requestCode, String[] permissions,
                                //                                          int[] grantResults)
                                // to handle the case where the user grants the permission. See the documentation
                                // for ActivityCompat#requestPermissions for more details.
                                return;
                            }
                            locationManager.removeUpdates(locationGpsListener);
                            locationManager.removeUpdates(locationNetworkListener);
                            locationManager = null;
                        }
                        locationGpsListener = null;
                        locationNetworkListener = null;

                        Message message = Message.obtain();
                        message.what = start;
                        sendMessageDelayed(message,locationTimeBigInterval);

                        break;
                    case start:
                        initPhoneLocationListener();//初始化
                        message = Message.obtain();
                        message.what = stop;
                        sendMessageDelayed(message, locationTimeSum);//停止定位
                        break;
                }
            }
        };

        Message message = Message.obtain();
        message.what = stop;
        handler.sendMessageDelayed(message, locationTimeSum);//停止定位
    }

    private void initPhoneLocationListener() {
        if (locationManager == null) {
            locationManager = (LocationManager) context.getSystemService(Context.LOCATION_SERVICE);
        }
        locationGpsListener = new LocationListener() {
            @Override
            public void onLocationChanged(Location location) {
                boolean betterLocation = isBetterLocation(location, bestLocation);
                if (betterLocation) {
                    bestLocation = location;
                }
                sevePhoneLocationContent(location);
            }

            @Override
            public void onStatusChanged(String provider, int status, Bundle extras) {

            }

            @Override
            public void onProviderEnabled(String provider) {

            }

            @Override
            public void onProviderDisabled(String provider) {

            }
        };
        locationNetworkListener = new LocationListener() {
            @Override
            public void onLocationChanged(Location location) {
                boolean betterLocation = isBetterLocation(location, bestLocation);
                if (betterLocation) {
                    bestLocation = location;
                }
                sevePhoneLocationContent(location);
            }

            @Override
            public void onStatusChanged(String provider, int status, Bundle extras) {

            }

            @Override
            public void onProviderEnabled(String provider) {

            }

            @Override
            public void onProviderDisabled(String provider) {

            }
        };

        if (ActivityCompat.checkSelfPermission(this, Manifest.permission.ACCESS_FINE_LOCATION) != PackageManager.PERMISSION_GRANTED
                && ActivityCompat.checkSelfPermission(this, Manifest.permission.ACCESS_COARSE_LOCATION) != PackageManager.PERMISSION_GRANTED) {
            // TODO: Consider calling
            //    ActivityCompat#requestPermissions
            // here to request the missing permissions, and then overriding
            //   public void onRequestPermissionsResult(int requestCode, String[] permissions,
            //                                          int[] grantResults)
            // to handle the case where the user grants the permission. See the documentation
            // for ActivityCompat#requestPermissions for more details.
            return;
        }
        locationManager.requestLocationUpdates(LOCATION_GPS, locationTimeSmallInterval, 0, locationGpsListener);
        locationManager.requestLocationUpdates(LOCATION_NETWORK, locationTimeSmallInterval, 0, locationNetworkListener);
        boolean betterLocation1 = isBetterLocation(locationManager.getLastKnownLocation(LOCATION_GPS), locationManager.getLastKnownLocation(LOCATION_NETWORK));
        if(betterLocation1) {
            bestLocation = locationManager.getLastKnownLocation(LOCATION_GPS);
        }
        boolean betterLocation2 = isBetterLocation(locationManager.getLastKnownLocation(LOCATION_NETWORK), bestLocation);
        if(betterLocation2) {
            bestLocation = locationManager.getLastKnownLocation(LOCATION_NETWORK);
        }

        sevePhoneLocationContent(bestLocation);

    }

    /** 判断是一个新的定位测量值是否比当前的定位修正值更好 时间--》准确性--》数据源
     * 这个方法代码我是在网络中找到的，具体地址和来源忘了，汗，原作者看到可留言
     * @param location  需要评估的新定位测量值
     * @param currentBestLocation  当前的定位修正值，也就是你想要用来跟新定位测量值比较的定位数据
     */
    protected boolean isBetterLocation(Location location, Location currentBestLocation) {
        if (currentBestLocation == null || location == null) {
            // 如果当前没有定位修正值，那么新的定位测量值肯定是更好的
            return true;
        }

        // 检查新的定位测量值是更新的数据还是更旧的数据
        long timeDelta = location.getTime() - currentBestLocation.getTime();
        boolean isSignificantlyNewer = timeDelta > TWO_MINUTES;
        boolean isSignificantlyOlder = timeDelta < -TWO_MINUTES;
        boolean isNewer = timeDelta > 0;

        // 如果新的定位测量值晚于当前的定位修正值两分钟，那么使用新的定位测量值，因为用户可能已经移动了
        if (isSignificantlyNewer) {
            return true;
            // 如果新的定位测量值早于当前定位修正值两分钟，那么新的定位测量值应该是过时的
        } else if (isSignificantlyOlder) {
            return false;
        }

        // 检查新的定位测量值精度是否更加精确
        int accuracyDelta = (int) (location.getAccuracy() - currentBestLocation.getAccuracy());
        boolean isLessAccurate = accuracyDelta > 0;
        boolean isMoreAccurate = accuracyDelta < 0;
        boolean isSignificantlyLessAccurate = accuracyDelta > 200;

        // 检查两个定位测量值是否来源于同一个定位数据源
        boolean isFromSameProvider = isSameProvider(location.getProvider(),
                currentBestLocation.getProvider());

        // 组合定位的及时性和准确度来评估定位的质量
        if (isMoreAccurate) {
            return true;
        } else if (isNewer && !isLessAccurate) {
            return true;
        } else if (isNewer && !isSignificantlyLessAccurate && isFromSameProvider) {
            return true;
        }
        return false;
    }

    /** 检查两个定位数据源是否是同一个数据源 */
    private boolean isSameProvider(String provider1, String provider2) {
        if (provider1 == null) {
            return provider2 == null;
        }
        return provider1.equals(provider2);
    }

    private void sevePhoneLocationContent(Location location){
        if(location == null){
            return;
        }
        StringBuffer buffer = new StringBuffer("");
        buffer.append("loctime:::");
        buffer.append(getFormatedDateTime(location.getTime()));
        buffer.append("   logtime:::");
        buffer.append(getFormatedDateTime(getMillisecond()));
        buffer.append("   provider:::");
        buffer.append(location.getProvider());
        buffer.append("   lat:::");
        buffer.append(location.getLatitude());
        buffer.append("   lng:::");
        buffer.append(location.getLongitude());

        phoneLocationList.add(buffer.toString());
        if(phoneLocationAdapter == null){
            phoneLocationAdapter = new PhoneLocationAdapter(context,phoneLocationList);
            lvList1.setAdapter(phoneLocationAdapter);
        }

        phoneLocationAdapter.setList(phoneLocationList);
        lvList2.setSelection(phoneLocationList.size() - 1);

        addContentToFile(savePhoneLOcationPath,buffer.toString());

    }


    private void initBaiduLocation(){
        baiduLocationAdapter = new BaiduLocationAdapter(context,baiduLocationList);
        lvList2.setAdapter(baiduLocationAdapter);

        BaiDuMapUtils.getIntance(context).startBackgroundBaiDuPositioning(new BaiduLocationCallBackListener() {
            @Override
            public void locationCallBackSuccess(BDLocation phoneLocationCallBackDto) {
                seveBaiduLocationContent(phoneLocationCallBackDto);
            }

            @Override
            public void locationCallBackFail(BDLocation phoneLocationCallBackDto) {
                seveBaiduLocationContent(phoneLocationCallBackDto);
            }
        },true,locationTimeSmallInterval, LocationClientOption.LocationMode.Hight_Accuracy);

        final int stop = 0;
        final int start = 1;
        final Handler handler = new Handler() {
            @Override
            public void handleMessage(Message msg) {
                super.handleMessage(msg);
                switch (msg.what) {
                    case stop:
                        BaiDuMapUtils.getIntance(context).stop();

                        Message message = Message.obtain();
                        message.what = start;
                        sendMessageDelayed(message,locationTimeBigInterval);

                        break;
                    case start:
                        BaiDuMapUtils.getIntance(context).startBackgroundBaiDuPositioning(new BaiduLocationCallBackListener() {
                            @Override
                            public void locationCallBackSuccess(BDLocation phoneLocationCallBackDto) {
                                seveBaiduLocationContent(phoneLocationCallBackDto);
                            }

                            @Override
                            public void locationCallBackFail(BDLocation phoneLocationCallBackDto) {
                                seveBaiduLocationContent(phoneLocationCallBackDto);
                            }
                        },true,locationTimeSmallInterval, LocationClientOption.LocationMode.Hight_Accuracy);
                        message = Message.obtain();
                        message.what = stop;
                        sendMessageDelayed(message, locationTimeSum);//停止定位
                        break;
                }
            }
        };

        Message message = Message.obtain();
        message.what = stop;
        handler.sendMessageDelayed(message, locationTimeSum);//停止定位
    }

    private void seveBaiduLocationContent(BDLocation location){
        if(location == null){
            return;
        }
        StringBuffer buffer = new StringBuffer("");
        buffer.append("loctime:::");
        buffer.append(location.getTime());
        buffer.append("   logtime:::");
        buffer.append(getFormatedDateTime(getMillisecond()));
        buffer.append("   provider:::");
        if (location.getLocType() == BDLocation.TypeGpsLocation){// GPS定位结果
            buffer.append("gps定位成功");
        } else if (location.getLocType() == BDLocation.TypeNetWorkLocation){// 网络定位结果
            buffer.append("网络定位成功");
        } else if (location.getLocType() == BDLocation.TypeOffLineLocation) {// 离线定位结果
            buffer.append("离线定位成功");
        } else if (location.getLocType() == BDLocation.TypeServerError) {
            buffer.append("服务端no");
        } else if (location.getLocType() == BDLocation.TypeNetWorkException) {
            buffer.append("no");
        } else if (location.getLocType() == BDLocation.TypeCriteriaException) {
            buffer.append("no");
        }
        buffer.append("   lat:::");
        buffer.append(location.getLatitude());
        buffer.append("   lng:::");
        buffer.append(location.getLongitude());

        baiduLocationList.add(buffer.toString());
        if(baiduLocationAdapter == null){
            baiduLocationAdapter = new BaiduLocationAdapter(context,baiduLocationList);
            lvList2.setAdapter(baiduLocationAdapter);
        }

        baiduLocationAdapter.setList(baiduLocationList);
        lvList2.setSelection(baiduLocationList.size() - 1);

        addContentToFile(saveBaiduLocationPath,buffer.toString());

    }



    /**
     * 获取当前时间的毫秒值
     * @return
     */
    private long getMillisecond(){
        return new Date().getTime();
    }

    /**
     * yyyy.MM.dd G 'at' hh:mm:ss z 如 '2002-1-1 AD at 22:10:59 PSD'
     * yy/MM/dd HH:mm:ss 如 '2002/1/1 17:55:00'
     * yy/MM/dd HH:mm:ss pm 如 '2002/1/1 17:55:00 pm'
     * yy-MM-dd HH:mm:ss 如 '2002-1-1 17:55:00'
     * yy-MM-dd HH:mm:ss am 如 '2002-1-1 17:55:00 am'
     * @return
     */
    private String getFormatedDateTime(long time) {
        SimpleDateFormat sDateFormat = new SimpleDateFormat("\"yyyy.MM.dd-HH:mm:ss.SSS\"");
        return sDateFormat.format(new Date(time + 0));
    }

    synchronized private void addContentToFile(String path, String content) {
        Log.d("contenLocationChange:::" , content);
        checkDirPathIsExistAndBuild(path);
        try {
            FileWriter fw = new FileWriter(new File(path), true);//以追加的模式将字符写入
            BufferedWriter bw = new BufferedWriter(fw);//又包裹一层缓冲流 增强IO功能
            bw.write(content);
            bw.flush();//将内容一次性写入文件
            bw.close();
        } catch (Exception e) {
        }
    }

    /**
     * 检查文件夹是否存在并创建文件夹
     *
     * @param path
     */
    private void checkDirPathIsExistAndBuild(String path) {
        checkDirPathIsExistAndBuild(new File(path));
    }

    /**
     * 检查文件夹是否存在并创建文件夹
     *
     * @param file
     */
    private void checkDirPathIsExistAndBuild(File file) {
        try {
            if (!file.isDirectory()) {
                if (!file.getParentFile().exists()) {
                    file.getParentFile().mkdirs();
                }
            } else {
                if (!file.exists()) {
                    file.mkdirs();
                }
            }
        } catch (Exception e) {
        }
    }

}

```

源码下载地址：https://github.com/Loren-Wang/GpsLocationTest