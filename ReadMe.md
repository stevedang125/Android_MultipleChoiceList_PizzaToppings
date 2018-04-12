## Create a simple order pizza toppings with the pop up dialog that let the user to select items:

Useful links:
```
Tutorial Video Link: Android multiple choice list dialog tutorial
https://www.youtube.com/watch?v=wfADRuyul04&t=14s

Found error/bugs: (working version)
mBuilder.setMultiChoiceItems(listItems, checkedItems, new DialogInterface.OnMultiChoiceClickListener() {
                    @Override
                    public void onClick(DialogInterface dialog, int position, boolean isChecked) {
                        // Check it the item is checked and if the string is not included in the array:
                        // if it's there remove it, if not add it.
                        if(isChecked){
                            // if the current item is not apart of the list, add it
                            // else remove it
                            if(!mUserItems.contains(position)){
                                mUserItems.add(position);
                            }
                        }
                        else{
                            if(mUserItems.contains(position)){
//                                mUserItems.remove(mUserItems.indexOf(position));
//                                Or this one works too
                                mUserItems.remove((Integer)(position));
                            }// end of if(isChecked)
                        }
                    // Debugging :)
                    // System.out.println(mUserItems);
                    }
                });

```
### 1) Java file:(ActivityMain.java)
```
package com.steve.multiplechoicelist;

import android.content.DialogInterface;
import android.support.v7.app.AlertDialog;
import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.TextView;

import java.util.ArrayList;

public class MainActivity extends AppCompatActivity {

    // Declare the Button and TV:
    Button mOrder;
    TextView mItemSelected;
    String [] listItems;
    boolean [] checkedItems;
    ArrayList<Integer> mUserItems = new ArrayList<>();

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        // Define the View:
        mOrder = (Button)findViewById(R.id.button_make_order);
        mItemSelected = (TextView)findViewById(R.id.tv_show_checked_items);

        // Create a string array that is going to hold the items in "res/values/strings.xml
//                <string-array name="shopping_item">
//                    <item>Onion</item>
//                    <item>Sausage</item>
//                    <item>Milk</item>
//                    <item>Garlic</item>
//                    <item>Beef</item>
//                    <item>Veggies</item>
//                    <item>Olive</item>
//                    <item>Cheese</item>
//                    <item>Tuna</item>
//                    <item>Mushrooms</item>
//                </string-array>
        // Declare a var that going to hold the items, line 13:
        // Boolean array that going to hold the checked items:
        // An arraylist to hold all the selected items

        // Parse the value of the list item:
        listItems = getResources().getStringArray(R.array.shopping_item);
        // Init. checked item boolean with the size of the listItems.
        checkedItems = new boolean[listItems.length];

        // Set onclick listener for the button:
        mOrder.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                // Create an alert dialog:
                AlertDialog.Builder mBuilder = new AlertDialog.Builder(MainActivity.this);
                mBuilder.setTitle("Items Available In The Shop");
                // Set multiple choice items:
                // bring in the list of items, the boolean array checkedItems and set onclick listener for it:
                mBuilder.setMultiChoiceItems(listItems, checkedItems, new DialogInterface.OnMultiChoiceClickListener() {
                    @Override
                    public void onClick(DialogInterface dialog, int position, boolean isChecked) {
                        // Check it the item is checked and if the string is not included in the array:
                        // if it's there remove it, if not add it.
                        if(isChecked){
                            // if the current item is not apart of the list, add it
                            // else remove it
                            if(!mUserItems.contains(position)){
                                mUserItems.add(position);
                            }
                        }
                        else{
                            if(mUserItems.contains(position)){
//                                mUserItems.remove(mUserItems.indexOf(position));
//                                Or this one works too
                                mUserItems.remove((Integer)(position));
                            }// end of if(isChecked)
                        }

                    // Debugging :)
                    // System.out.println(mUserItems);


                    }
                });


                mBuilder.setCancelable(false);
                mBuilder.setPositiveButton("Save", new DialogInterface.OnClickListener() {
                    @Override
                    public void onClick(DialogInterface dialog, int which) {
                        // Create an empty string:
                        // Loop through the list of mUserItems, set the item in the list to the
                        // temp item here:
                        String item = "";
                        // Loop through the arraylist mUserItems:
                        // get the selected items out of the original listItems
                        for(int i = 0; i < mUserItems.size(); i++){
                            item = item + listItems[mUserItems.get(i)];
                            // Make sure the last item won't have a comma =D
                            if(i != mUserItems.size() - 1){
                                item = item + ", ";
                            }
                        }

                        // Call the tv that created
                        mItemSelected.setText(item);

                    }
                });


                mBuilder.setNegativeButton("Dismiss", new DialogInterface.OnClickListener() {
                    @Override
                    public void onClick(DialogInterface dialog, int which) {
                        dialog.dismiss();
                    }
                });


                mBuilder.setNeutralButton("Clear All", new DialogInterface.OnClickListener() {
                    @Override
                    public void onClick(DialogInterface dialog, int which) {
                        // Loop through the checked items, false on everything ^^
                        for(int i = 0; i < checkedItems.length; i++){
                            checkedItems[i] = false;
                            // Clear items inside the user list
                            mUserItems.clear();
                            // Clear the items inside the textView
                            mItemSelected.setText("");
                        }
                    }
                });

                // Create the mBuilder then show the dialog
                AlertDialog mDialog = mBuilder.create();
                mDialog.show();



            }
        });

    }
}

```

### 2) .xml files:(activity_main.xml and strings.xml)
```
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context="com.steve.multiplechoicelist.MainActivity">


    <Button
        android:id="@+id/button_make_order"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_above="@+id/tv_show_checked_items"
        android:layout_centerHorizontal="true"
        android:layout_marginBottom="55dp"
        android:text="Make Order" />

    <TextView
        android:id="@+id/tv_show_checked_items"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_alignParentBottom="true"
        android:layout_centerHorizontal="true"
        android:layout_marginBottom="221dp"
        android:textSize="25dp"
        android:textStyle="italic"
        android:textColor="@color/colorPrimary"
        android:gravity="center"

        />

</RelativeLayout>

==============================================================================
<resources>
    <string name="app_name">MultipleChoiceList</string>

    <string-array name="shopping_item">
        <item>Onion</item>
        <item>Sausage</item>
        <item>Milk</item>
        <item>Garlic</item>
        <item>Beef</item>
        <item>Veggies</item>
        <item>Olive</item>
        <item>Cheese</item>
        <item>Tuna</item>
        <item>Mushrooms</item>
    </string-array>

</resources>

```