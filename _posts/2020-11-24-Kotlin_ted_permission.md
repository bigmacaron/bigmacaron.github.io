---
title: " Kotlin ted_permission"
date: 2020-11-24 
categories:  Image processing
---

# 코틀린 정리

&nbsp;


## ted_permission  
&nbsp;

ted님이 만든 라이브러리 사용예:  
  
``` AndroidManifest ``` 에 작성

```
<uses-permission android:name="android.permission.CAMERA"/>
<uses-permission android:name="android.permission.RECORD_AUDIO" />
```

```build.gradle(app)``` 에 작성

```
implementation 'gun0912.ted:tedpermission:2.2.2'
```

```MainActivity``` 에 작성
```
import com.gun0912.tedpermission.PermissionListener
import com.gun0912.tedpermission.TedPermission

class MainActivity : AppCompatActivity() {

    private val TAG = "Camera"

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        button.setOnClickListener {
            checkPermission()

        }

    }
// ---------------------권한설정-----------------------
    private val permission = object : PermissionListener {
        override fun onPermissionGranted() {
            Log.d(TAG, "Permission: Success")
            Toast.makeText(this@MainActivity,"허용됨.",Toast.LENGTH_SHORT).show()
            //허용했을때,Todo
        }

        override fun onPermissionDenied(deniedPermissions: MutableList<String>?) {
            Toast.makeText(this@MainActivity,"권한 허용을 하지 않으면 서비스를 이용할 수 없습니다.",Toast.LENGTH_SHORT).show()
            Log.d(TAG, "Permission: Failed")
            finish()
        }
    }
    private fun checkPermission() {
        TedPermission.with(this)
                .setPermissionListener(permission)
                .setRationaleMessage("녹화를 위하여 권한을 허용해주세요.")
                .setDeniedMessage("권한이 거부되었습니다. 설정 > 권한에서 허용해주세요.")
                .setPermissions(
                        android.Manifest.permission.CAMERA, // 카메라 권한
                        android.Manifest.permission.RECORD_AUDIO, // 오디오 녹음 권한
//                        android.Manifest.permission.WRITE_EXTERNAL_STORAGE // 저장 권한
                )
                .check()
    }

}
```
버튼클릭시 권한 허용 질문  
``` activity_main```에 작성
```
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <Button
        android:id="@+id/button"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Button"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

</androidx.constraintlayout.widget.ConstraintLayout>
```
버튼 한개 생성

