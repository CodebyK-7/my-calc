package com.example.mycalculator;

import android.os.Bundle;  
import android.view.View;  
import android.widget.Button;      
import android.widget.EditText;    
import android.widget.LinearLayout;  
import androidx.appcompat.app.AppCompatActivity;  

public class MainActivity extends AppCompatActivity {

    private EditText display;
    private String input = "";
    private double firstNumber = 0;
    private String operator = "";

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);

        // Create Main Layout (Vertical Orientation)
        LinearLayout layout = new LinearLayout(this);
        layout.setOrientation(LinearLayout.VERTICAL);
        layout.setPadding(20, 20, 20, 20);

        // Create Display (EditText)
        display = new EditText(this);
        display.setTextSize(24);
        display.setEnabled(false);
        layout.addView(display);

        // Create Number Buttons (0-9)
        int[] numberIds = {0, 1, 2, 3, 4, 5, 6, 7, 8, 9};
        for (int i = 1; i <= 9; i += 3) {
            LinearLayout row = new LinearLayout(this);
            row.setOrientation(LinearLayout.HORIZONTAL);
            for (int j = 0; j < 3; j++) {
                Button btn = createButton(String.valueOf(i + j), numberIds[i + j]);
                row.addView(btn);
            }
            layout.addView(row);
        }

        // Create Last Row (0, Clear, Equal)
        LinearLayout lastRow = new LinearLayout(this);
        lastRow.setOrientation(LinearLayout.HORIZONTAL);
        lastRow.addView(createButton("0", 0));
        lastRow.addView(createButton("C", -1)); // Clear Button
        lastRow.addView(createButton("=", -2)); // Equal Button
        layout.addView(lastRow);

        // Create Operator Row (+, -, ×, ÷)
        LinearLayout operatorRow = new LinearLayout(this);
        operatorRow.setOrientation(LinearLayout.HORIZONTAL);
        operatorRow.addView(createButton("+", -3));
        operatorRow.addView(createButton("-", -4));
        operatorRow.addView(createButton("×", -5));
        operatorRow.addView(createButton("÷", -6));
        layout.addView(operatorRow);

        // Set Layout as Content View
        setContentView(layout);
    }

    private Button createButton(String text, int id) {
        Button btn = new Button(this);
        btn.setText(text);
        btn.setTextSize(20);
        btn.setPadding(20, 10, 20, 10);
        btn.setOnClickListener(view -> onButtonClick(id, text));
        return btn;
    }

    private void onButtonClick(int id, String text) {
        if (id >= 0) { // Number Clicked
            input += text;
            display.setText(input);
        } else if (id == -1) { // Clear Button
            input = "";
            firstNumber = 0;
            operator = "";
            display.setText("");
        } else if (id == -2) { // Equal Button
            calculate();
        } else { // Operator Clicked
            if (!input.isEmpty()) {
                firstNumber = Double.parseDouble(input);
                operator = text;
                input = "";
            }
        }
    }

    private void calculate() {
        if (!input.isEmpty() && !operator.isEmpty()) {
            double secondNumber = Double.parseDouble(input);
            double result = 0;

            switch (operator) {
                case "+":
                    result = firstNumber + secondNumber;
                    break;
                case "-":
                    result = firstNumber - secondNumber;
                    break;
                case "×":
                    result = firstNumber * secondNumber;
                    break;
                case "÷":
                    if (secondNumber != 0) {
                        result = firstNumber / secondNumber;
                    } else {
                        display.setText("Error");
                        return;
                    }
                    break;
            }

            display.setText(String.valueOf(result));
            input = String.valueOf(result);
            operator = "";
        }
    }
}
