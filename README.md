#activity_main#
<img width="959" alt="{06AE31CD-9F78-46C3-84EB-215E6D05A62F}" src="https://github.com/user-attachments/assets/95035e69-9ee5-4002-a0c6-c6673fdea15c" /><?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:padding="16dp">

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/label_pilih_tanggal_waktu"
        android:textSize="18sp"
        android:layout_marginBottom="16dp" />

    <Button
        android:id="@+id/dateTimeButton"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/button_pilih_tanggal_waktu" />

    <TextView
        android:id="@+id/selectedDateTimeTextView"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginTop="8dp"
        android:layout_marginBottom="16dp"
        android:text="@string/label_tanggal_waktu_terpilih_default" />

    <View
        android:layout_width="match_parent"
        android:layout_height="1dp"
        android:background="@android:color/darker_gray"
        android:layout_marginBottom="16dp" />

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/label_masukkan_nomor_telepon"
        android:textSize="18sp"
        android:layout_marginBottom="16dp" />

    <EditText
        android:id="@+id/phoneNumberEditText"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="@string/hint_nomor_telepon"
        android:inputType="phone" />

    <Button
        android:id="@+id/submitPhoneNumberButton"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/button_submit_nomor_telepon"
        android:layout_marginTop="16dp" />

    <TextView
        android:id="@+id/submittedPhoneNumberTextView"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginTop="8dp"
        android:text="@string/label_nomor_telepon_dimasukkan_default" />

</LinearLayout>

#MainActivity.xml#

package com.example.inputcontrolapp

import android.app.DatePickerDialog
import android.app.TimePickerDialog
import android.os.Bundle
import android.widget.Button
import android.widget.EditText
import android.widget.TextView
import androidx.appcompat.app.AppCompatActivity
import java.text.SimpleDateFormat
import java.util.Calendar
import java.util.Locale

class MainActivity : AppCompatActivity() {

    private lateinit var dateTimeButton: Button
    private lateinit var selectedDateTimeTextView: TextView
    private lateinit var calendar: Calendar

    private lateinit var phoneNumberEditText: EditText
    private lateinit var submitPhoneNumberButton: Button
    private lateinit var submittedPhoneNumberTextView: TextView

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        // Inisialisasi dan logika untuk Date Time Picker
        dateTimeButton = findViewById(R.id.dateTimeButton)
        selectedDateTimeTextView = findViewById(R.id.selectedDateTimeTextView)
        calendar = Calendar.getInstance()

        dateTimeButton.setOnClickListener { showDateTimePicker() }

        // Inisialisasi dan logika untuk Input Nomor Telepon
        phoneNumberEditText = findViewById(R.id.phoneNumberEditText)
        submitPhoneNumberButton = findViewById(R.id.submitPhoneNumberButton)
        submittedPhoneNumberTextView = findViewById(R.id.submittedPhoneNumberTextView)

        submitPhoneNumberButton.setOnClickListener {
            val phoneNumber = phoneNumberEditText.text.toString()
            submittedPhoneNumberTextView.text = getString(R.string.label_nomor_telepon_dimasukkan_default).replace("-", phoneNumber)
        }
    }

    private fun showDateTimePicker() {
        val datePickerDialog = DatePickerDialog(
            this,
            { _, year, monthOfYear, dayOfMonth ->
                calendar.set(Calendar.YEAR, year)
                calendar.set(Calendar.MONTH, monthOfYear)
                calendar.set(Calendar.DAY_OF_MONTH, dayOfMonth)
                showTimePicker()
            },
            calendar.get(Calendar.YEAR),
            calendar.get(Calendar.MONTH),
            calendar.get(Calendar.DAY_OF_MONTH)
        )
        datePickerDialog.show()
    }

    private fun showTimePicker() {
        val timePickerDialog = TimePickerDialog(
            this,
            { _, hourOfDay, minute ->
                calendar.set(Calendar.HOUR_OF_DAY, hourOfDay)
                calendar.set(Calendar.MINUTE, minute)
                updateSelectedDateTime()
            },
            calendar.get(Calendar.HOUR_OF_DAY),
            calendar.get(Calendar.MINUTE),
            true // is24HourView
        )
        timePickerDialog.show()
    }

    private fun updateSelectedDateTime() {
        val sdf = SimpleDateFormat("yyyy-MM-dd HH:mm", Locale.getDefault())
        selectedDateTimeTextView.text = getString(R.string.label_tanggal_waktu_terpilih_default).replace("-", sdf.format(calendar.time))
    }
}

#string.xml#
<resources>
    <string name="app_name">InputControl</string>
    <string name="label_pilih_tanggal_waktu">Pilih Tanggal dan Waktu</string>
    <string name="button_pilih_tanggal_waktu">Pilih Tanggal dan Waktu</string>
    <string name="label_tanggal_waktu_terpilih_default">Tanggal dan Waktu Terpilih: -</string>
    <string name="label_masukkan_nomor_telepon">Masukkan Nomor Telepon</string>
    <string name="hint_nomor_telepon">Nomor Telepon</string>
    <string name="button_submit_nomor_telepon">Submit Nomor Telepon</string>
    <string name="label_nomor_telepon_dimasukkan_default">Nomor Telepon yang Dimasukkan: -</string>
</resources>
