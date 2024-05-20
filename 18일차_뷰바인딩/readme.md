## 뷰 파인딩

---

안드로이드 스튜디오에서 뷰 바인딩(View Binding)은 XML 레이아웃 파일과 코드 간의 상호 작용을 더 쉽게 만들어주는 기능입니다. 뷰 바인딩을 사용하면 findViewById()를 사용하지 않고도 XML 레이아웃의 뷰에 안전하게 접근할 수 있습니다. 이를 통해 코드의 안전성과 가독성이 높아집니다. 

다음은 안드로이드 스튜디오에서 뷰 바인딩을 설정하고 사용하는 방법에 대한 단계별 가이드입니다.

### 1. 뷰 바인딩 설정

프로젝트의 `build.gradle` 파일에서 뷰 바인딩을 활성화합니다.

**`build.gradle (Module: app)`** 파일에 다음 코드를 추가합니다:
```gradle
android {
    ...
    viewBinding {
        enabled = true
    }
}
```

### 2. 뷰 바인딩 사용

뷰 바인딩을 활성화하면, 각 XML 레이아웃 파일에 대한 바인딩 클래스가 생성됩니다. 예를 들어, `activity_main.xml` 파일이 있다면, `ActivityMainBinding` 클래스가 자동으로 생성됩니다.

#### 2.1 레이아웃 파일 생성

**`activity_main.xml`**:
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/main"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    tools:context=".MainActivity">

    <Button
        android:id="@+id/visibleBtn"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="visible" />

    <TextView
        android:id="@+id/targetView"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:background="#FF0000"
        android:text="hello world"
        android:textColor="#FFFFFF" />

    <Button
        android:id="@+id/invisibleBtn"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="invisible" />

</LinearLayout>
```

#### 2.2 바인딩 객체 사용

**`MainActivity.kt`**:
```kotlin
package com.fpkm9999.view_binding_01

import android.os.Bundle
import android.view.View
import androidx.appcompat.app.AppCompatActivity
import com.fpkm9999.view_binding_01.databinding.ActivityMainBinding

class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        // ViewBinding 객체를 생성하여 레이아웃 인플레이션
        val binding = ActivityMainBinding.inflate(layoutInflater)

        // 생성된 ViewBinding 객체의 루트 뷰를 setContentView로 설정하여 화면에 표시
        setContentView(binding.root)

        // visibleBtn 클릭 시 targetView를 보이게 설정
        binding.visibleBtn.setOnClickListener {
            binding.targetView.visibility = View.VISIBLE
        }

        // invisibleBtn 클릭 시 targetView를 보이지 않게 설정
        binding.invisibleBtn.setOnClickListener {
            binding.targetView.visibility = View.INVISIBLE
        }
    }
}

```

### 장점

1. **안전성**: 뷰의 ID가 잘못되었을 때 컴파일 시점에 오류를 발견할 수 있습니다.
2. **성능**: `findViewById()` 호출을 줄여 성능을 개선할 수 있습니다.
3. **가독성**: 코드가 간결해지고 가독성이 향상됩니다.

### 추가 팁

- 뷰 바인딩은 프래그먼트에서도 사용할 수 있습니다. 프래그먼트에서 뷰 바인딩을 사용할 때는 뷰가 생성되고 파괴될 때 바인딩 객체를 적절히 관리해야 합니다.
- 리사이클러뷰와 함께 사용할 때는 뷰 홀더에서 뷰 바인딩을 사용할 수 있습니다.


### 프래그먼트에서 뷰 바인딩 사용 예제

**`ExampleFragment.kt`**:
```kotlin
package com.fpkm9999.view_binding_01

import android.os.Bundle
import android.view.LayoutInflater
import android.view.View
import android.view.ViewGroup
import androidx.fragment.app.Fragment
import com.fpkm9999.view_binding_01.databinding.FragmentExampleBinding

class ExampleFragment : Fragment() {

    private var _binding: FragmentExampleBinding? = null
    private val binding get() = _binding!!

    override fun onCreateView(
        inflater: LayoutInflater, container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View? {
        // 바인딩 객체 획득 및 레이아웃 인플레이션
        _binding = FragmentExampleBinding.inflate(inflater, container, false)
        return binding.root
    }

    override fun onDestroyView() {
        super.onDestroyView()
        // 뷰가 파괴될 때 바인딩 객체를 정리
        _binding = null
    }
}
```



