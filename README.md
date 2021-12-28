**A Detailed report on the stickiness estimation from Diffusion Limited
Aggregation (DLA) images**

*\_by Srinivasa Teja*

In order to extend the initial study that has been done earlier based on 4
indices, due to its limitations in the terms of data available for fitting,
predicting and also to optimize the algorithm for automatic data collection for
a greater number of DLA simulations in a robust manner, this analysis is the
extension of the previous report.

The term sensitivity index presented in the previous study is used as Feature1
and also a new feature that seems to highly correlate with the stickiness is
also studied in this case.

**Details of the simulations:**

-   The analysis is done for 101x101 matrix

-   Range of N values: 100 to 1500

-   Range of K values: 0.001 to 0.9

-   Total No. of Observations: 530

-   Test size: 424 (80% of total).

The code file named “DLA_v6.0.ipynb” takes the input matrix size, the number of
particles N, stickiness k, and generates a simulated image along with the data
presented in the below figure for every 100 step size.

In order to illustrate the above, for a given N, say N = 1500, the output will
contain the following data for the simulations of N = 100,200,300,…1500 .

![image](https://user-images.githubusercontent.com/71938185/147536312-81f66906-0963-49c1-b0c0-7b2fadbff4f0.png)


Fig.1: This figure shows the typical data and its structure obtained from
running the simulation.

**Features used for the analysis:**

**Feature 1 :** No of non-zero pixels that would be present in the image after
applying a sobel edge detection filter which will be under the name “edge” and
the normalized version of it will be “edge/N”

**Feature 2:** No of zeros around each pixel of value 1 for a given DLA
simulated image, labelled as “zeros_around” and its normalized version is
labelled as “ zeros_around/N “.

Note that Feature 1 and Feature 2 can be obtained from the code titled
“Estimation_of \_k-v2.0.ipynb”. The variation of k with respect to these two
features is as shown below.

There is no convergence observed
by just using feature 1 and 2 without normalizing them using N can be seen in
Fig2 and Fig3 respectively. For feature 1 the ranges are spanning around (100 to
5000) and for feature 2 they are spanning in between (100 to 7000) this might
significantly increase with further increase in N.

![image](https://user-images.githubusercontent.com/71938185/147536421-78d202fe-15ff-4800-bcef-557008b7dff2.png)Fig.2: Feature 1 vs k

![image](https://user-images.githubusercontent.com/71938185/147536538-cb1a6e94-7844-4aa7-8800-d82008b564bb.png)
Fig.3: Feature 2 vs k

So, in order to confine that feature values with in a less varying extent, they
have been normalized by dividing with N, plots are generated for the same are
shown below.

![image](https://user-images.githubusercontent.com/71938185/147536554-eac7143c-efa5-4c75-915d-2ab043f6669c.png)

Fig.4: Normalized Feature 1 vs k

![image](https://user-images.githubusercontent.com/71938185/147536579-4c4ee17e-1058-411b-b2d3-f6bc26890202.png)

Fig.5: Normalized Feature 2 vs k

From the above plots, the data points are much scalable to work for fitting and
we can also observe the convergence of each feature with the increase in N value
for a given stickiness and has observed the same across all k values, for
smaller N values the variation is significant to take into account.

The next step is to split the data into testing and training sets, and to fit
the curve on test set thus by obtaining a model. Three fits were decided and
made based on Fig:4 and Fig:5. They are Linear, first order, and second order
with respect to normalized feature1 and normalized feature2 separately.

The following are the plots and
results as shown.

![image](https://user-images.githubusercontent.com/71938185/147536625-1b941a80-e89c-40fa-9e89-110168451c19.png)Fig.6: Different models fitted
and tested for the data Normalized Feature 1 and k, the predictions on test set
is shown in this figure

![image](https://user-images.githubusercontent.com/71938185/147536637-efe76266-4a19-4802-8b86-bf35fe227ba9.png)
Fig.7: Different models fitted for the data Normalized Feature 2 and k, the
predictions on test set is shown in this figure

When compared with linear and second degree fitting, the third degree curve is
fitting better so we can go for that to estimate k.

The results are shown in the following table.

| **Case**             | **RMSE_train** | **R2_train** | **RMSE_test** | **R2_test** |
|----------------------|----------------|--------------|---------------|-------------|
| **Linear_case1**     | 0.117          | 0.833        | 0.106         | 0.851       |
| **2nd degree_case1** | 0.081          | 0.92109      | 0.0797        | 0.916       |
| **3rd degree_case1** | 0.08           | 0.9214       | 0.0787        | 0.918       |
| **Linear_case2**     | 0.144          | 0.747        | 0.14          | 0.75        |
| **2nd degree_case2** | 0.076          | 0.928        | 0.08          | 0.919       |
| **3rd degree_case2** | 0.0715         | 0.937        | 0.0727        | 0.933       |

But even within third degree curve there is a variation in the features with
respect to a particular k, especially when N is low, we can see a number of
outliers in 3rd degree fit in both the cases.

So, we cannot completely generalize a single static model for predicting k at
least for smaller values of N. So, one optimal solution is to generate a dynamic
model. For this first estimate the N from the image that is given for
prediction, then find the data that is in considerable range around that N, fit
a 3rd order curve using either normalized feature 1 or normalized feature 2. But
3rd degree curve fit using normalized feature 2 is giving better results when
compared with all.

**Further points to be considered for a more robust analysis:**

-   Effects of different sizes of Matrices.

-   More number of N.

-   Optimal way to speed up the process.

-   More number of simulations for each case in order to average the
    observations.

    -   This is because even for a given k and N, there is a variation that has
        been observed each time in 50 simulations that have been done on 101x101
        matrix, N =100, k =0.15 and the feature variations is as shown below.

        ![image](https://user-images.githubusercontent.com/71938185/147536661-497d863d-dc2a-4291-be18-ac8db955f771.png)
        
        ![image](https://user-images.githubusercontent.com/71938185/147536831-d01d8971-3051-4e1c-a3f7-f9af1b0dc504.png)


-   Usage of different kernel sizes in sobel edge detection for feature 1 used
    in the study to be analysed. As this is done based on 5x5 filter only.
