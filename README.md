# Algorithm_Analysis_HW_1
My Previous work on our homework

/*                                   */
/*Three-Way Merge Sort (W/O Graphics)*/
/*                                   */

import java.math.*;
import java.util.*;
public class ThreeWayMergeSort {
    private int[] array;
    private int[] tempMergArr;
    private int length;

    public static void main(String a[]){

        //Randomly elected numbers, and setup for merge sort
        Random rndm = new Random();
        int[] inputArray = new int[99];
        for (int i = 0; i < inputArray.length; i++){
            inputArray[i] = rndm.nextInt(100);
        }
        ThreeWayMergeSort sorter = new ThreeWayMergeSort();
        sorter.sort(inputArray);
        for(int i:inputArray){
            System.out.print(i + " ");
        }
    }
    //Intializes recursive method for sorting
    public void sort(int inputArray[]) {
        this.array = inputArray;
        this.length = inputArray.length;
        this.tempMergArr = new int[length];
        merge(0, length - 1);
    }
    //splits array into smaller arrays
    private void merge(int lowerIndex, int higherIndex) {

        if (lowerIndex < higherIndex) {
            int index1 = lowerIndex + (higherIndex - lowerIndex) / 3;
            int index2 = lowerIndex + 2 * (higherIndex - lowerIndex) / 3;
            // Sorts the left side of the array
            merge(lowerIndex, index1);
            // sorts the middle of the array
            merge(index1 + 1, index2);
            // sorts the right side of the array
            merge(index2 + 1, higherIndex);
            // Merges both sides
            merge(lowerIndex, index1, index2, higherIndex);
        }
    }

    private void merge(int lowerIndex, int index1,int index2, int higherIndex) {
        //create a temporary array for sorting
        for (int i = lowerIndex; i <= higherIndex; i++) {
            tempMergArr[i] = array[i];
        }
        int leftIndex = lowerIndex;
        int middleIndex = index1 + 1;
        int rightIndex = index2 + 1;
        int arrayIndex = lowerIndex;
        //Select which side is smaller
        //Case 1: all 3 arrays have remaining numbers
        while (leftIndex <= index1 && rightIndex <= higherIndex && middleIndex <= index2) {

            if (tempMergArr[leftIndex] <= tempMergArr[rightIndex]
                    && tempMergArr[leftIndex] <= tempMergArr[middleIndex]) { //choose left and add to new array
                array[arrayIndex] = tempMergArr[leftIndex];
                leftIndex++;

            } else if (tempMergArr[middleIndex] <= tempMergArr[rightIndex]
                    && tempMergArr[middleIndex] <= tempMergArr[leftIndex]) {
                array[arrayIndex] = tempMergArr[middleIndex];         //choose right and add to new array
                middleIndex++;
            }

            else {
                array[arrayIndex] = tempMergArr[rightIndex];
                rightIndex++;
            }
            arrayIndex++;
        }
        //Case 2: remaining/ remaining /complete
        while (leftIndex <= index1 && middleIndex > index2 && rightIndex <= higherIndex){

                if (tempMergArr[leftIndex] <= tempMergArr[rightIndex]) { //choose left and add to new array
                    array[arrayIndex] = tempMergArr[leftIndex];
                    leftIndex++;
                } else if (tempMergArr[rightIndex] <= tempMergArr[leftIndex]){
                    array[arrayIndex] = tempMergArr[rightIndex];         //choose right and add to new array
                    rightIndex++;
                }
                arrayIndex++;
        }

        //Case 3: remaining/ complete/ remaining
        while (leftIndex <= index1 && middleIndex <= index2 && rightIndex > higherIndex){
            if (tempMergArr[leftIndex] <= tempMergArr[middleIndex]) { //choose left and add to new array
                array[arrayIndex] = tempMergArr[leftIndex];
                leftIndex++;
            } else if (tempMergArr[middleIndex] <= tempMergArr[leftIndex]){
                array[arrayIndex] = tempMergArr[middleIndex];         //choose right and add to new array
                middleIndex++;
            }
            arrayIndex++;
        }
        //Case 4: complete/ remaining/ remaining
        while (leftIndex > index1 && middleIndex <= index2 && rightIndex <= higherIndex){
            if (tempMergArr[middleIndex] <= tempMergArr[rightIndex]) { //choose left and add to new array
                array[arrayIndex] = tempMergArr[middleIndex];
                middleIndex++;
            } else if (tempMergArr[rightIndex] <= tempMergArr[middleIndex]){
                array[arrayIndex] = tempMergArr[rightIndex];         //choose right and add to new array
                rightIndex++;
            }
            arrayIndex++;
        }
        //Copy remaining temp array to new array
        while (leftIndex <= index1) {
            array[arrayIndex] = tempMergArr[leftIndex];
            arrayIndex++;
            leftIndex++;
        }
        while (middleIndex <= index2){
            array[arrayIndex] = tempMergArr[middleIndex];
            arrayIndex++;
            middleIndex++;
        }

    }
}


/*                          */
/* Quick Sort (W/O Graphics)*/
/*                          */

import java.util.Arrays;
import java.util.Random;
public class QuickSorter {
    public static void main(String[] args) {
        Random rndm = new Random();
        int[] test = new int[100];

        //creates a array random
        for (int i = 0; i < test.length; i++) {
            test[i] = rndm.nextInt(1000);
        }

        System.out.print("Unsorted array: ");
        printArray(test);

        quickSort(test, 0, test.length - 1);

        System.out.print("\nSorted array:\t");
        printArray(test);

    }

    //Prints an array
    private static void printArray(int[] list) {
        for (int i = 0; i < list.length; i++) {
            System.out.print(list[i] + "\t");
        }
    }

    //recursively calls itself to sort the array
    private static void quickSort(int[] list, int lower, int higher){
        if(lower < higher){ //Base Case
            int index = partition(list, lower, higher);
            quickSort(list, lower, index-1);
            quickSort(list, index+1, higher);
        }
    }

    //Runs quick sorting
    private static int partition (int[] list, int lower, int higher){
        int pivot;
        int lowerIndex = lower-1;

        //choose pivot
       if ((higher - lower) < 3)
            pivot = list[higher];
       else {
           int pivotIndex = median(new int[]{list[lower], list[lower + 1], list[lower + 2]}, 0, 2);
           pivot = list[pivotIndex];
           swap(list, pivotIndex, higher);
       }

        //sorts array around the pivot
        for(int i = lower; i < higher; i++){
            if(list[i] <= pivot){
                lowerIndex++;
                swap(list, lowerIndex, i);
            }
        }
        if(lowerIndex+1 <= higher)
        swap(list, lowerIndex+1, higher);
        return lowerIndex+1;
    }

    //swaps two position in an array
    private static void swap(int[] list, int posA, int posB){
        int temp;

        temp = list[posA];
        list[posA] = list[posB];
        list[posB] = temp;
    }

    //returns the index of the median of the first three of an array
    public static int median(int[] array, int left, int right) {
        int center = (left + right) / 2;
        int[] indexTracker = new int[] {left, center, right};

        if (array[left] > array[center]) {
            swap(array, left, center);
            swap(indexTracker, left, center);
        }

        if (array[left] > array[right]) {
            swap(array, left, right);
            swap(indexTracker, left, right);
        }

        if (array[center] > array[right]) {
            swap(array, center, right);
            swap(indexTracker, center, right);
        }

        swap(array, center, right - 1);
        swap(indexTracker, center, right-1);
        return indexTracker[right - 1];
    }
}


/*                                   */
/*Binary Insertion Sort (W/ Graphics)*/
/*                                   */

import java.util.Random;

import javafx.application.Application;
import javafx.scene.Group;
import javafx.scene.control.*;
import javafx.scene.Scene;
import javafx.scene.layout.BorderPane;
import javafx.scene.layout.GridPane;
import javafx.scene.layout.Pane;
import javafx.scene.text.Text;
import javafx.stage.Stage;
import javafx.scene.paint.Color;
import javafx.scene.shape.Rectangle;

public class BinaryInsertionSort extends Application{

    //Uses a combination of binary search and insertion sort to sort an array effeciently
    //then creates a visual display to show the effects of the array
    static Rectangle[] boxGraph;
    static int[] test;
    static int[] arrayTestTemp;

    static int numbers = 20;
    int sceneWidth = 315;
    int sceneHeight = 315;
    int graphBase = 250;
    int barOffSet = 15;
    static int intersectionCounter = 0;
    Color selectedColor = Color.RED;
    Color unselectedColor = Color.GRAY;
    Pane pane;
    static Text[] text;

    public static void main(String[] args){
        text = new Text[numbers];
        for (int i = 0; i < text.length; i++)
            text[i] = new Text("");

        /*int[] nums = new int[] { 90, 1, 5 ,8, 2 ,12, 32, 86 , 0, 2};

        for(int i = 0; i < nums.length; i++)
            insertionSort(nums);
        printArray(nums);*/
        launch(args);
    }
    @Override
    public void start(Stage primaryStage){

        pane = new Pane();
        Random rndm = new Random();
        arrayTestTemp = new int[numbers];
        test = new int[numbers];
        boxGraph = new Rectangle[numbers];

        //Fills arrays with numbers and boxes for those numbers
        for(int i = 0; i < boxGraph.length; i++){
            test[i] = rndm.nextInt(20) + 1;
            arrayTestTemp[i] = test[i];
            boxGraph[i] = new Rectangle(10, test[i]*10);
            text[i].setText(test[i] + "");

            boxGraph[i].setFill(Color.RED);
            boxGraph[i].setY(graphBase - boxGraph[i].getHeight());
            boxGraph[i].setX(barOffSet*(i+1));

            text[i].setY(graphBase - boxGraph[i].getHeight()-10);
            text[i].setX(barOffSet*(i+1));

            pane.getChildren().addAll(boxGraph[i], text[i]);
        }

        printArray(test);

        //Creates a step button to slowly itterate through insertion sort using binary search
        Button step = new Button("Step");
        step.setLayoutX(sceneWidth/2 - 30);
        step.setLayoutY(sceneHeight - 30);
        step.setOnAction(e -> {
                    insertionSort(test);
                    //printArray(test);
                    for(int i = 0; i < boxGraph.length; i++) {
                        boxGraph[i].setHeight(test[i] * 10);
                        boxGraph[i].setY(graphBase - boxGraph[i].getHeight());

                        text[i].setText(test[i] + "");
                        text[i].setY(graphBase - boxGraph[i].getHeight()-10);
                        text[i].setX(barOffSet*(i+1));
                    }
            intersectionCounter++;
        });
        pane.getChildren().add(step);

        //Creates reset button
        Button reset = new Button("Reset");
        reset.setLayoutX(sceneWidth/2 + 20);
        reset.setLayoutY(sceneHeight - 30);
        reset.setOnAction(e -> {
            refillArray();
            for(int i = 0; i < boxGraph.length; i++) {
                boxGraph[i].setHeight(test[i] * 10);
                boxGraph[i].setY(graphBase - boxGraph[i].getHeight());

                text[i].setText(test[i] + "");
                text[i].setY(graphBase - boxGraph[i].getHeight()-10);
                text[i].setX(barOffSet*(i+1));
            }

            intersectionCounter = 0;
        });
        pane.getChildren().add(reset);

        // Create a scene and place it in the stage
        Scene scene = new Scene(pane, sceneWidth, sceneHeight);
        primaryStage.setTitle("Insertion Sort"); // Set the stage title
        primaryStage.setScene(scene); // Place the scene in the stage
        primaryStage.show(); // Display the stage
    }

    //Sorts array using insertion sort
    public static void insertionSort(int array[]) {
        int i = intersectionCounter;


        if ( i < array.length) {
            int pointer = array[i];
            int previous = i - 1;

            int targetIndex = binarySearch(array, 0, i , pointer);

            if(targetIndex == -1){
                System.out.print("Number not found\n");
            }
            else {
                int tempTarget = array[i];
                int c = targetIndex;
                int shiftCounter = i;

                while (shiftCounter > c) {
                    array[shiftCounter] = array[shiftCounter-1];
                    shiftCounter--;
                }
                array[targetIndex] = tempTarget;
            }
            //intersectionCounter++;
        }
        else
            printArray(array);
    }

    public static int binarySearch(int[] array, int lower, int higher, int target) {
        int mid = lower + (higher - lower) / 2;

        if (higher>lower) {
                if (array[mid] < target)
                    return binarySearch(array, mid + 1, higher, target);
                else if (array[mid] > target)
                    return binarySearch(array, lower, mid , target);
                else
                    return mid;
        }
        else
            return mid;
    }

    //prints out in full, any integer array in full
    public static void printArray (int[] array){
        for(int i = 0; i < array.length; i++){
            System.out.print(array[i] + "\t");
        }
        System.out.println("");
    }

    public static void updateRectangles (Rectangle[] boxes){
        for(int i = 0; i < boxGraph.length; i++){
            boxGraph[i] = new Rectangle(10, test[i]*10);
            boxGraph[i].setY(225 - boxGraph[i].getHeight());
        }
    }

    public static void refillArray(){
        Random rndm = new Random();
        for ( int i = 0; i < test.length; i++){
            test[i] = rndm.nextInt(20);
        }
    }
    public static void refreshArray(){
        for ( int i = 0; i < test.length; i++){
            test[i] = arrayTestTemp[i];
        }
    }
}


