# PDF Viewer App

This is a simple Android app that allows users to pick and view PDF files. It utilizes the [Pdf-Viewer library](https://github.com/afreakyelf/Pdf-Viewer/) for rendering PDF content within the app.

## Features

- Allows users to pick a PDF file from the device storage.
- Displays the selected PDF in the app using a custom view.

## Setup Instructions

### Step 1: Add Dependencies

Add the following dependency to your app's `build.gradle` file:

```gradle
dependencies {
    implementation("io.github.afreakyelf:Pdf-Viewer:2.1.1")
}
```

### Step 2: Update XML Layout

In your `activity_main.xml`, set up a button for picking the PDF and a `PdfRendererView` for displaying it.

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:gravity="center"
    android:orientation="vertical"
    tools:context=".MainActivity">

    <Button
        android:id="@+id/pickPdfButton"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginStart="30dp"
        android:layout_marginEnd="30dp"
        android:backgroundTint="@color/colorPrimary"
        android:text="Pick PDF"
        android:textColor="#ffffff"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

    <com.rajat.pdfviewer.PdfRendererView
        android:id="@+id/pdfView"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        app:pdfView_divider="@drawable/pdf_viewer_divider"
        app:pdfView_showDivider="false" />
</LinearLayout>
```

### Step 3: Implement MainActivity

In your `MainActivity`, implement the logic for picking a PDF file and displaying it.

```java
package com.eazly.pdfviewjava;

import android.content.Intent;
import android.net.Uri;
import android.os.Bundle;
import android.view.View;
import androidx.activity.result.ActivityResultLauncher;
import androidx.activity.result.contract.ActivityResultContracts;
import androidx.appcompat.app.AppCompatActivity;
import com.eazly.pdfviewjava.databinding.ActivityMainBinding;
import java.io.FileNotFoundException;

public class MainActivity extends AppCompatActivity {
    ActivityMainBinding binding;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        binding = ActivityMainBinding.inflate(getLayoutInflater());
        setContentView(binding.getRoot());

        binding.pickPdfButton.setOnClickListener(v -> {
            launchFilePicker();
        });
    }

    private void launchFilePicker() {
        Intent intent = new Intent(Intent.ACTION_OPEN_DOCUMENT);
        intent.addCategory(Intent.CATEGORY_OPENABLE);
        intent.setType("application/pdf");
        filePicker.launch(intent);
    }

    private final ActivityResultLauncher<Intent> filePicker = registerForActivityResult(
            new ActivityResultContracts.StartActivityForResult(),
            result -> {
                if (result.getResultCode() == RESULT_OK) {
                    Intent data = result.getData();
                    if (data != null && data.getData() != null) {
                        Uri selectedFileUri = data.getData();
                        try {
                            binding.pdfView.initWithUri(selectedFileUri);
                        } catch (FileNotFoundException e) {
                            throw new RuntimeException(e);
                        }
                    }
                }
            }
    );
}
```

### Step 4: Sync and Run

After completing the steps, sync your project and run the app on an Android device or emulator.

## Credits

This project is powered by the [Pdf-Viewer library](https://github.com/afreakyelf/Pdf-Viewer) created by [afreakyelf](https://github.com/afreakyelf).
