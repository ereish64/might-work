Procedures:
1. Capture JPEG Images from Camera. Ideal Prefix is <name of Class>_<imagenum>.JPEG
2. Place Images in darknet\BBox-Label-Tool-master\Images\001 Folder (deleting any other images already there).
3. Run main.py of BBox Label Tool and place boxes around object of interest (Current information does not indicate if you should be consistent in which box corner you select).
4. Create the folders darknet\scripts\images\<name of class>\, darknet\scripts\labels\<name of class>\, 
5. Copy the Images from BBox-Label-Tool-master\Images\001 folder to darknet\scripts\images\<name of class>\, and darknet\scripts\labels\<name of class>_original\
6. Copy the Labels from BBox-Label-Tool-master\Labels\001 folder to darknet\scripts\labels\<name of class>_original\
7. Modify darknet\scripts\convert.py. Change classes to <name of class> (from above). Change mypath and outpath to point to the labels folders you just created.
8. Run the convert.py script. This should result in the <name of class>_list.txt file being created. It appears that if you want to do multiple classes you should modify the script for each one and then merge the resulting files. For these directions we are focusing on a single class.
9. Modify the darknet\src\yolo.c 
9.a. Change #define CLASSNUM 1 to #define CLASSNUM <number of classes>
9.b. Change char *voc_names[] = {"frccube"}; to char *voc_names[] = {"<name of Class 1>","<name of Class 2>",...};
9.c. NEEDs TO BE CHECKED: Change char *train_images = "scripts/frccube_list.txt"; to char *train_images = "scripts/<name of class>_list.txt"; This could also be the combined list you created in step 8 if you have multiple classes. Also this may need a full path to function. The original path in the code did not exist for reference but had this format.
9.d. NEEDS TO BE CHECKED: *backup_directory = "model/"; might need to be changed to a full path. In the original code it was a full path. The model folder is provided for you to target.
10. Modify the darknet\src\yolo_kernels.cu and change #define CLS_NUM 1 to #define CLS_NUM <number of classes>
11. Copy the darknet\cfg\YOLO.cfg to darknet\cfg\YOLO_<num Classes>class_box<side number>.cfg 7 is default. 11 is more accurate. (Note: Our testing started with the yolo_2class_box11.cfg file so some of the parameters are different.)
12. Modify the darknet\cfg\YOLO_<num Classes>class_box<side number>.cfg file so that classes=<number of classes> and side=<side number>. Then calculate the output value by (5 x 2 + <number of classes>) x <side number> x <side number>. ex. (5 x 2 + 1) x 11 x 11 = 1331. This number is the Final [connected] output Line 218.
12. Run the following in the top level \darknet\ directory. I've placed the extraction.conv.weights in that directory. You may need to move it to a correct location but without testing I can not be sure. Run: ./darknet yolo train cfg/yolo.cfg extraction.conv.weights 
