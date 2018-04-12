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

Fast links:

1) All the java files:

2) ALl the .xml files:



```