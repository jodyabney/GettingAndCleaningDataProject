Tidy_Data2.txt Codebook Definition
==========================


#### Description
A space-delimited text file containing a tidy data set consisting of the following fields:

1. subject
2. Mean_tBodyAcc.mean...X
3. Mean_tBodyAcc.mean...Y
4. Mean_tBodyAcc.mean...Z
5. Mean_tBodyAcc.std...X
6. Mean_tBodyAcc.std...Y
7. Mean_tBodyAcc.std...Z
8. Mean_tGravityAcc.mean...X
9. Mean_tGravityAcc.mean...Y
10. Mean_tGravityAcc.mean...Z
11. Mean_tGravityAcc.std...X
12. Mean_tGravityAcc.std...Y
13. Mean_tGravityAcc.std...Z
14. Mean_tBodyAccJerk.mean...X
15. Mean_tBodyAccJerk.mean...Y
16. Mean_tBodyAccJerk.mean...Z
17. Mean_tBodyAccJerk.std...X
18. Mean_tBodyAccJerk.std...Y
19. Mean_tBodyAccJerk.std...Z
20. Mean_tBodyGyro.mean...X
21. Mean_tBodyGyro.mean...Y
22. Mean_tBodyGyro.mean...Z
23. Mean_tBodyGyro.std...X
24. Mean_tBodyGyro.std...Y
25. Mean_tBodyGyro.std...Z
26. Mean_tBodyGyroJerk.mean...X
27. Mean_tBodyGyroJerk.mean...Y
28. Mean_tBodyGyroJerk.mean...Z
29. Mean_tBodyGyroJerk.std...X
30. Mean_tBodyGyroJerk.std...Y
31. Mean_tBodyGyroJerk.std...Z
32. Mean_tBodyAccMag.mean..
33. Mean_tBodyAccMag.std..
34. Mean_tGravityAccMag.mean..
35. Mean_tGravityAccMag.std..
36. Mean_tBodyAccJerkMag.mean..
37. Mean_tBodyAccJerkMag.std..
38. Mean_tBodyGyroMag.mean..
39. Mean_tBodyGyroMag.std..
40. Mean_tBodyGyroJerkMag.mean..
41. Mean_tBodyGyroJerkMag.std..
42. Mean_fBodyAcc.mean...X
43. Mean_fBodyAcc.mean...Y
44. Mean_fBodyAcc.mean...Z
45. Mean_fBodyAcc.std...X
46. Mean_fBodyAcc.std...Y
47. Mean_fBodyAcc.std...Z
48. Mean_fBodyAcc.meanFreq...X
49. Mean_fBodyAcc.meanFreq...Y
50. Mean_fBodyAcc.meanFreq...Z
51. Mean_fBodyAccJerk.mean...X
52. Mean_fBodyAccJerk.mean...Y
53. Mean_fBodyAccJerk.mean...Z
54. Mean_fBodyAccJerk.std...X
55. Mean_fBodyAccJerk.std...Y
56. Mean_fBodyAccJerk.std...Z
57. Mean_fBodyAccJerk.meanFreq...X
58. Mean_fBodyAccJerk.meanFreq...Y
59. Mean_fBodyAccJerk.meanFreq...Z
60. Mean_fBodyGyro.mean...X
61. Mean_fBodyGyro.mean...Y
62. Mean_fBodyGyro.mean...Z
63. Mean_fBodyGyro.std...X
64. Mean_fBodyGyro.std...Y
65. Mean_fBodyGyro.std...Z
66. Mean_fBodyGyro.meanFreq...X
67. Mean_fBodyGyro.meanFreq...Y
68. Mean_fBodyGyro.meanFreq...Z
69. Mean_fBodyAccMag.mean..
70. Mean_fBodyAccMag.std..
71. Mean_fBodyAccMag.meanFreq..
72. Mean_fBodyBodyAccJerkMag.mean..
73. Mean_fBodyBodyAccJerkMag.std..
74. Mean_fBodyBodyAccJerkMag.meanFreq..
75. Mean_fBodyBodyGyroMag.mean..
76. Mean_fBodyBodyGyroMag.std..
77. Mean_fBodyBodyGyroMag.meanFreq..
78. Mean_fBodyBodyGyroJerkMag.mean..
79. Mean_fBodyBodyGyroJerkMag.std..
80. Mean_fBodyBodyGyroJerkMag.meanFreq..
81. training_label
82. activity

#### Data Cleanup Activities Undertaken to Produce tidy_dat2.txt:

1. Read the source data files (activity labels, features, test, training)
2. Merge the test and training data into a single dataset
3. Define the readings columns to be used in the tidy dataset (e.g. those that are "means" or "standard deviations" of their data readings)
4. Set up tidy_data set based on the required columns including the mapping of the activity text values based on the training_label data column values
5. Create an empty tidy_data2 dataset based on the tidy_data dataset to get the column structure
9. Get the unique subject values and loop through each subject based on the unique activity values to calculate the averages of each readings column
10. Append the averages by student by activity onto the tidy_data2 dataset
11. Remap the activity text based on the training_label data
12. Relabel the now averaged reading columns by prepending their names with "Mean_"
