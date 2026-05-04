# Tutorial RecyclerView Android

Tutorial ini akan membimbing Anda membuat aplikasi daftar mahasiswa menggunakan RecyclerView di Android Studio.

## 1. Membuat Project Baru
1. Buka **Android Studio**.
2. Pilih **New Project**.
3. Pilih template **Empty Views Activity** (Gunakan ini agar mendapatkan file XML layout secara otomatis).
4. Masukkan konfigurasi berikut:
   - **Name**: `RecyclerView`
   - **Package name**: `com.example.recyclerview`
   - **Save location**: Sesuaikan dengan folder Anda.
   - **Language**: `Kotlin`
   - **Minimum SDK**: Pilih API 24 atau yang direkomendasikan.
5. Klik **Finish** dan tunggu proses sinkronisasi Gradle selesai.

## 2. Cara Membuat File di Android Studio

### Membuat File Kotlin (Class/Data Class)
1. Buka folder `app` -> `java` -> `com.example.recyclerview`.
2. **Klik Kanan** pada folder package `com.example.recyclerview`.
3. Pilih **New** -> **Kotlin Class/File**.
4. Ketik nama file (misal: `Mahasiswa`) dan tekan **Enter**.

### Membuat File Layout (XML)
1. Buka folder `app` -> `res` -> `layout`.
2. **Klik Kanan** pada folder `layout`.
3. Pilih **New** -> **Layout Resource File**.
4. Ketik nama file (misal: `item_mahasiswa`) dan klik **OK**.

---

## 3. Implementasi Kode Lengkap

### A. Membuat Model Data: [Mahasiswa.kt](app/src/main/java/com/example/recyclerview/Mahasiswa.kt)
Buat file baru bernama `Mahasiswa.kt` dan masukkan kode berikut:

```kotlin
package com.example.recyclerview

data class Mahasiswa(val nama: String, val nim: String)
```

### B. Membuat Layout Item: [item_mahasiswa.xml](app/src/main/res/layout/item_mahasiswa.xml)
Buat file layout baru bernama `item_mahasiswa.xml` di folder `res/layout`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:orientation="vertical"
    android:padding="16dp">

    <TextView
        android:id="@+id/tvNama"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:textSize="18sp"
        android:textStyle="bold" />

    <TextView
        android:id="@+id/tvNim"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:textSize="14sp" />

</LinearLayout>
```

### C. Membuat Adapter: [MahasiswaAdapter.kt](app/src/main/java/com/example/recyclerview/MahasiswaAdapter.kt)
Buat file `MahasiswaAdapter.kt` untuk mengatur bagaimana data dimasukkan ke dalam RecyclerView:

```kotlin
package com.example.recyclerview

import android.view.LayoutInflater
import android.view.View
import android.view.ViewGroup
import android.widget.TextView
import androidx.recyclerview.widget.RecyclerView

class MahasiswaAdapter(private val daftarMahasiswa: List<Mahasiswa>) :
    RecyclerView.Adapter<MahasiswaAdapter.ViewHolder>() {

    class ViewHolder(view: View) : RecyclerView.ViewHolder(view) {
        val tvNama: TextView = view.findViewById(R.id.tvNama)
        val tvNim: TextView = view.findViewById(R.id.tvNim)
    }

    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): ViewHolder {
        val view = LayoutInflater.from(parent.context)
            .inflate(R.layout.item_mahasiswa, parent, false)
        return ViewHolder(view)
    }

    override fun onBindViewHolder(holder: ViewHolder, position: Int) {
        val mhs = daftarMahasiswa[position]
        holder.tvNama.text = mhs.nama
        holder.tvNim.text = mhs.nim
    }

    override fun getItemCount() = daftarMahasiswa.size
}
```

### D. Membuat Layout Activity: [activity_main.xml](app/src/main/res/layout/activity_main.xml)
Buka atau buat `activity_main.xml` dan tambahkan widget RecyclerView:

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/main"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".RecyclerViewActivity">

    <androidx.recyclerview.widget.RecyclerView
        android:id="@+id/rvMahasiswa"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintEnd_toEndOf="parent" />

</androidx.constraintlayout.widget.ConstraintLayout>
```

### E. Membuat Activity Utama: [MainActivity.kt](app/src/main/java/com/example/recyclerview/MainActivity.kt)
Langkah terakhir, hubungkan data dengan adapter di dalam `MainActivity`:

```kotlin
package com.example.recyclerview

import android.os.Bundle
import androidx.activity.enableEdgeToEdge
import androidx.appcompat.app.AppCompatActivity
import androidx.core.view.ViewCompat
import androidx.core.view.WindowInsetsCompat
import androidx.recyclerview.widget.LinearLayoutManager
import androidx.recyclerview.widget.RecyclerView

class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        enableEdgeToEdge()
        setContentView(R.layout.activity_main)

        val rvMahasiswa = findViewById<RecyclerView>(R.id.rvMahasiswa)
        
        // Data Dummy
        val data = listOf(
            Mahasiswa("Andi", "123456"),
            Mahasiswa("Budi", "654321"),
            Mahasiswa("Citra", "112233")
        )

        // Setup LayoutManager dan Adapter
        rvMahasiswa.layoutManager = LinearLayoutManager(this)
        rvMahasiswa.adapter = MahasiswaAdapter(data)

        ViewCompat.setOnApplyWindowInsetsListener(findViewById(R.id.main)) { v, insets ->
            val systemBars = insets.getInsets(WindowInsetsCompat.Type.systemBars())
            v.setPadding(systemBars.left, systemBars.top, systemBars.right, systemBars.bottom)
            insets
        }
    }
}
```

---
**Catatan**: Pastikan nama package dan ID layout sudah sesuai dengan yang Anda buat di Android Studio.
