# Module 2 / Regression

# **Introduction**

*Regression* is a form of machine learning that is used to predict a numeric *label* based on an item's *features*. For example, an automobile sales company might use the characteristics of a car (such as engine size, number of seats, mileage, and so on) to predict its likely selling price. In this case, the characteristics of the car are the features, and the selling price is the label.

Regression is an example of a *supervised* machine learning technique in which you train a model using data that includes both the features and known values for the label, so that the model learns to *fit* the feature combinations to the label. Then, after training has been completed, you can use the trained model to predict labels for new items for which the label is unknown.

You can use Microsoft Azure Machine Learning designer to create regression models by using a drag and drop visual interface, without needing to write any code.

In this module, you'll learn how to:

- Use Azure Machine Learning designer to train a regression model.
- Use a regression model for inferencing.
- Deploy a regression model as a service.

To complete this module, you'll need a Microsoft Azure subscription. If you don't already have one, you can sign up for a free trial at [https://azure.microsoft.com](https://azure.microsoft.com/).

# **Create an Azure Machine Learning workspace**

Azure Machine Learning is a cloud-based platform for building and operating machine learning solutions in Azure. It includes a wide range of features and capabilities that help data scientists prepare data, train models, publish predictive services, and monitor their usage. One of these features is a visual interface called *designer*, that you can use to train, test, and deploy machine learning models without writing any code.

## **Create an Azure Machine Learning workspace**

To use Azure Machine Learning, you create a *workspace* in your Azure subscription. You can then use this workspace to manage data, compute resources, code, models, and other artifacts related to your machine learning workloads.

**Note**

This module is one of many that make use of an Azure Machine Learning workspace, including the other modules in the **[Microsoft Azure AI Fundamentals: Explore visual tools for machine learning](https://docs.microsoft.com/en-us/learn/paths/create-no-code-predictive-models-azure-machine-learning/)** learning path. If you are using your own Azure subscription, you may consider creating the workspace once and reusing it in other modules. Your Azure subscription will be charged a small amount for data storage as long as the Azure Machine Learning workspace exists in your subscription, so we recommend you delete the Azure Machine Learning workspace when it is no longer required.

If you don't already have one, follow these steps to create a workspace:

1. Sign into the [Azure portal](https://portal.azure.com/) using your Microsoft credentials.
2. Select **＋Create a resource**, search for *Machine Learning*, and create a new **Azure Machine Learning** resource with an *Azure Machine Learning* plan. Use the following settings:
    - **Subscription**: *Your Azure subscription*
    - **Resource group**: *Create or select a resource group*
    - **Workspace name**: *Enter a unique name for your workspace*
    - **Region**: *Select the closest geographical region*
    - **Storage account**: *Note the default new storage account that will be created for your workspace*
    - **Key vault**: *Note the default new key vault that will be created for your workspace*
    - **Application insights**: *Note the default new application insights resource that will be created for your workspace*
    - **Container registry**: None (*one will be created automatically the first time you deploy a model to a container*)
3. Select **Review + create**. Wait for your workspace to be created (it can take a few minutes). Then go to it in the portal.
4. On the **Overview** page for your workspace, launch Azure Machine Learning Studio (or open a new browser tab and navigate to [https://ml.azure.com](https://ml.azure.com/)), and sign into Azure Machine Learning Studio using your Microsoft account.
5. In Azure Machine Learning Studio, toggle the ☰ icon at the top left to view the various pages in the interface. You can use these pages to manage the resources in your workspace.

You can manage your workspace using the Azure portal, but for data scientists and Machine Learning operations engineers, Azure Machine Learning Studio provides a more focused user interface for managing workspace resources.

# **Create compute resources**

To train and deploy models using Azure Machine Learning designer, you need compute targets to run the training process. You will also use these compute targets to test the trained model after its deployment.

## **Create compute targets**

Compute targets are cloud-based resources on which you can run model training and data exploration processes.

In [Azure Machine Learning studio](https://ml.azure.com/), expand the left pane by selecting the three lines at the top left of the screen. View the **Compute** page (under **Manage**). You manage the compute targets for your data science activities in the studio. There are four kinds of compute resource that you can create:

- **Compute Instances**: Development workstations that data scientists can use to work with data and models.
- **Compute Clusters**: Scalable clusters of virtual machines for on-demand processing of experiment code.
- **Inference Clusters**: Deployment targets for predictive services that use your trained models.
- **Attached Compute**: Links to existing Azure compute resources, such as Virtual Machines or Azure Databricks clusters.

**Note**

Compute instances and clusters are based on standard Azure virtual machine images. For this module, the *Standard_DS11_v2* image is recommended to achieve the optimal balance of cost and performance. If your subscription has a quota that does not include this image, choose an alternative image; but bear in mind that a larger image may incur higher cost and a smaller image may not be sufficient to complete the tasks. Alternatively, ask your Azure administrator to extend your quota.

1. On the **Compute Instances** tab, add a new compute instance with the following settings:
    - **Compute name**: *enter a unique name*
    - **Location**: *Note the default. The compute instance's location is always created in the same location as the workspace.*
    - **Virtual Machine type**: CPU
    - **Virtual Machine size**:
        - Choose **Select from all options**
        - Search for and select **Standard_DS11_v2**
    - Select **Create**
2. While the compute instance is being created, switch to the **Compute Clusters** tab, and add a new compute cluster with the following settings:
    - **Location**: *Select the same as your workspace. If that location is not listed, choose the one closest to you*
    - **Virtual Machine tier**: Dedicated
    - **Virtual Machine type**: CPU
    - **Virtual Machine size**:
        - Choose **Select from all options**
        - Search for and select **Standard_DS11_v2**
    - Select **Next**
    - **Compute name**: *enter a unique name*
    - **Minimum number of nodes**: 0
    - **Maximum number of nodes**: 2
    - **Idle seconds before scale down**: 120
    - **Enable SSH access**: Unselected
    - Select **Create**

**Tip**

After you finish the entire module, be sure to follow the **Clean Up** instructions at the end of the module to stop your compute resources. Stop your compute resources to ensure your subscription won't be charged.

The compute targets take some time to be created. You can move onto the next unit while you wait.

# **Explore data**

To train a regression model, you need a dataset that includes historical *features*, characteristics of the entity for which you want to make a prediction. You also need known *label* values, the numeric value that you want to train a model to predict.

## **Create a pipeline**

To use the Azure Machine Learning designer, you create a *pipeline* that you use to train a machine learning model. This pipeline starts with the dataset from which you want to train the model.

1. In [Azure Machine Learning studio](https://ml.azure.com/), expand the left pane by selecting the three lines at the top left of the screen. View the **Designer** page (under **Author**), and select **+** to create a new pipeline.
2. At the top right-hand side of the screen, select **Settings**. If the **Settings** pane is not visible, select the **⚙** icon next to the pipeline name at the top.
3. In **Settings**, you must specify a compute target on which to run the pipeline. Under **Select compute type**, select **Compute cluster**. Then under **Select Azure ML *compute-type***, select the compute cluster you created previously.
4. In **Settings**, under **Draft Details**, change the draft name (**Pipeline-Created-on-*date***) to **Auto Price Training**.
5. Select the *close icon* on the top right of the **Settings** pane to close the pane.

## **Add and explore a dataset**

In this module, you train a regression model that predicts the price of an automobile based on its characteristics. Azure Machine Learning includes a sample dataset that you can use for this model.

1. Next to the pipeline name on the left, select the button **>>** to expand the panel if it is not already expanded. The panel should open by default to the **Asset Library** pane, indicated by the books icon at the top of the panel. Note that there is a search bar to locate assets. Below the **Tags** and **Filter** buttons, there are two icons next to each other. Hover your mouse over the first cylinder icon (on the left) to see that it represents **Data Assets**. Hover your mouse over the second chart icon (on the right) to see that it represents **Components**.
    
    ![https://docs.microsoft.com/en-us/learn/wwl-data-ai/create-regression-model-azure-machine-learning-designer/media/designer-asset-library-data.png](https://docs.microsoft.com/en-us/learn/wwl-data-ai/create-regression-model-azure-machine-learning-designer/media/designer-asset-library-data.png)
    
2. Click on the cylinder icon for **Data Assets**. Drag the **Automobile price data (Raw)** dataset onto the canvas.
3. Right-click (Ctrl+click on a Mac) the **Automobile price data (Raw)** dataset on the canvas, and on the **Outputs** menu, click **Dataset output** on the *Preview data* graph icon.
4. Review the schema of the data. Note that you can see the distributions of the various columns as histograms.
5. Scroll to the right of the dataset until you see the **Price** column, which is the label that your model predicts.
6. Select the column header for the **price** column and view the details that are displayed in the pane to the right. These include various statistics for the column values, and a histogram with the distribution of the column values.
7. Scroll back to the left and select the **normalized-losses** column header. Then review the statistics for this column. Note there are quite a few missing values in this column. Missing values limit the column's usefulness for predicting the **price** label so you might want to exclude it from training.
8. View the statistics for the **bore**, **stroke**, and **horsepower** columns. Note the number of missing values. These columns have fewer missing values than **normalized-losses**, so they might still be useful in predicting **price** once you exclude the rows where the values are missing from training.
9. Compare the values in the **stroke**, **peak-rpm**, and **city-mpg** columns. These columns are all measured in different scales, and it is possible that the larger values for **peak-rpm** might bias the training algorithm and create an over-dependency on this column compared to columns with lower values, such as **stroke**. Typically, data scientists mitigate this possible bias by *normalizing* the numeric columns so they're on the similar scales.
10. Close the **Automobile price data (Raw) result visualization** window so that you can see the dataset on the canvas like this:
    
    ![https://docs.microsoft.com/en-us/learn/wwl-data-ai/create-regression-model-azure-machine-learning-designer/media/dataset.png](https://docs.microsoft.com/en-us/learn/wwl-data-ai/create-regression-model-azure-machine-learning-designer/media/dataset.png)
    

## **Add data transformations**

You typically apply data transformations to prepare the data for modeling. In the case of the automobile price data, you add transformations to address the issues you identified when you explored the data.

1. In the **Asset Library** pane on the left, click on the squares icon to access **Components**, which contain a wide range of modules you can use for data transformation and model training. You can also use the search bar to quickly locate modules.
    
    ![https://docs.microsoft.com/en-us/learn/wwl-data-ai/create-regression-model-azure-machine-learning-designer/media/designer-asset-library-components.png](https://docs.microsoft.com/en-us/learn/wwl-data-ai/create-regression-model-azure-machine-learning-designer/media/designer-asset-library-components.png)
    
2. Find the **Select Columns in Dataset** module and drag it to the canvas, below the **Automobile price data (Raw)** module. Then connect the output at the bottom of the **Automobile price data (Raw)** module to the input at the top of the **Select Columns in Dataset** module, like this:
    
    ![https://docs.microsoft.com/en-us/learn/wwl-data-ai/create-regression-model-azure-machine-learning-designer/media/dataset-select-columns.png](https://docs.microsoft.com/en-us/learn/wwl-data-ai/create-regression-model-azure-machine-learning-designer/media/dataset-select-columns.png)
    
3. Select the **Select Columns in Dataset** module, and in its **Settings** pane on the right, select **Edit column**. Then in the **Select columns** window, select **By name** and use the **+** links to add all columns other than **normalized-losses**, like this:
    
    ![https://docs.microsoft.com/en-us/learn/wwl-data-ai/create-regression-model-azure-machine-learning-designer/media/select-columns.png](https://docs.microsoft.com/en-us/learn/wwl-data-ai/create-regression-model-azure-machine-learning-designer/media/select-columns.png)
    

In the rest of this exercise, you go through steps to create a pipeline that looks like this:

![https://docs.microsoft.com/en-us/learn/wwl-data-ai/create-regression-model-azure-machine-learning-designer/media/data-transforms.png](https://docs.microsoft.com/en-us/learn/wwl-data-ai/create-regression-model-azure-machine-learning-designer/media/data-transforms.png)

Follow the remaining steps, use the image for reference as you add and configure the required modules.

1. Find a **Clean Missing Data** module and place it under the **Select Columns in Dataset** module on the canvas. Then connect the output from the **Select Columns in Dataset** module to the input of the **Clean Missing Data** module.
2. Select the **Clean Missing Data** module, and in the settings pane on the right, click **Edit column**. Then in the **Select columns** window, select **With rules**, in the **Include** list select **Column names**, in the box of column names enter **bore**, **stroke**, and **horsepower** like this:
    
    ![https://docs.microsoft.com/en-us/learn/wwl-data-ai/create-regression-model-azure-machine-learning-designer/media/clean-missing-values.png](https://docs.microsoft.com/en-us/learn/wwl-data-ai/create-regression-model-azure-machine-learning-designer/media/clean-missing-values.png)
    
3. With the **Clean Missing Data** module still selected, in the settings pane, set the following configuration settings:
    - **Minimum missing value ratio**: 0.0
    - **Maximum missing value ratio**: 1.0
    - **Cleaning mode**: Remove entire row
4. Find a **Normalize Data** module and drag it to the canvas, below the **Clean Missing Data** module. Then connect the left-most output from the **Clean Missing Data** module to the input of the **Normalize Data** module.
5. Select the **Normalize Data** module and view its settings. Note that it requires you to specify the transformation method and the columns to be transformed. Then, set the transformation to **MinMax**. Apply a rule to edit the columns to include the following **Column names**:
    - **symboling**
    - **wheel-base**
    - **length**
    - **width**
    - **height**
    - **curb-weight**
    - **engine-size**
    - **bore**
    - **stroke**
    - **compression-ratio**
    - **horsepower**
    - **peak-rpm**
    - **city-mpg**
    - **highway-mpg**
    
    ![https://docs.microsoft.com/en-us/learn/wwl-data-ai/create-regression-model-azure-machine-learning-designer/media/normalize-rules.png](https://docs.microsoft.com/en-us/learn/wwl-data-ai/create-regression-model-azure-machine-learning-designer/media/normalize-rules.png)
    

## **Run the pipeline**

To apply your data transformations, you must run the pipeline as an experiment.

1. Ensure that your pipeline looks similar to this image:
    
    ![https://docs.microsoft.com/en-us/learn/wwl-data-ai/create-regression-model-azure-machine-learning-designer/media/data-transforms.png](https://docs.microsoft.com/en-us/learn/wwl-data-ai/create-regression-model-azure-machine-learning-designer/media/data-transforms.png)
    
2. Select **Submit**, and run the pipeline as a new experiment named **mslearn-auto-training** on your compute cluster.
3. Wait for the run to finish, which might take 5 minutes or more.
    
    ![https://docs.microsoft.com/en-us/learn/wwl-data-ai/create-regression-model-azure-machine-learning-designer/media/completed-job.png](https://docs.microsoft.com/en-us/learn/wwl-data-ai/create-regression-model-azure-machine-learning-designer/media/completed-job.png)
    
    Notice that the left hand panel is now on the **Submitted Jobs** pane. You will know when the run is complete because the status of the job will change to **Complete**.
    
4. When the run has completed, click on **Job Details**. You will be taken to another window which will show the modules like this:
    
    ![https://docs.microsoft.com/en-us/learn/wwl-data-ai/create-regression-model-azure-machine-learning-designer/media/normalize-complete.png](https://docs.microsoft.com/en-us/learn/wwl-data-ai/create-regression-model-azure-machine-learning-designer/media/normalize-complete.png)
    

## **View the transformed data**

The dataset is now prepared for model training.

1. Select the completed **Normalize Data** module, and in its **Settings** pane on the right, on the **Outputs + logs** tab, select the **Preview data** icon for the **Transformed dataset**.
2. View the data. Note that the **normalized-losses** column has been removed, all rows contain data for **bore**, **stroke**, and **horsepower**, and the numeric columns you selected have been normalized to a common scale.
3. Close the normalized data result visualization.

# **Create and run a training pipeline**

After you've used data transformations to prepare the data, you can use it to train a machine learning model.

## **Add training modules**

It's common practice to train the model using a subset of the data, while holding back some data with which to test the trained model. This enables you to compare the labels that the model predicts with the actual known labels in the original dataset.

In this exercise, you're going to work through steps to extend the **Auto Price Training** pipeline as shown here:

![https://docs.microsoft.com/en-us/learn/wwl-data-ai/create-regression-model-azure-machine-learning-designer/media/train-score.png](https://docs.microsoft.com/en-us/learn/wwl-data-ai/create-regression-model-azure-machine-learning-designer/media/train-score.png)

Follow the steps below, using the image above for reference as you add and configure the required modules.

1. Open the **Auto Price Training** pipeline you created in the previous unit if it's not already open.
2. In the **Asset Library** pane on the left, in **Components** section, find and drag a **Split Data** module onto the canvas under the **Normalize Data** module. Then connect the *Transformed Dataset* (left) output of the **Normalize Data** module to the input of the **Split Data** module.
    
    **Tip**
    
    Use the search bar to quickly locate modules.
    
3. Select the **Split Data** module, and configure its settings as follows:
    - **Splitting mode**: Split Rows
    - **Fraction of rows in the first output dataset**: 0.7
    - **Random seed**: 123
    - **Stratified split**: False
4. Expand the **Model Training** section in the pane on the left, and drag a **Train Model** module to the canvas, under the **Split Data** module. Then connect the *Result dataset1* (left) output of the **Split Data** module to the *Dataset* (right) input of the **Train Model** module.
5. The model we're training will predict the **price** value, so select the **Train Model** module and modify its settings to set the **Label column** to **price** (matching the case and spelling exactly!)
6. The **price** label the model will predict is a numeric value, so we need to train the model using a *regression* algorithm. Expand the **Machine Learning Algorithms** section, and under **Regression**, drag a **Linear Regression** module to the canvas, to the left of the **Split Data** module and above the **Train Model** module. Then connect its output to the **Untrained model** (left) input of the **Train Model** module.
    
    **Note**
    
    There are multiple algorithms you can use to train a regression model. For help choosing one, take a look at the **[Machine Learning Algorithm Cheat Sheet for Azure Machine Learning designer](https://aka.ms/mlcheatsheet)**.
    
7. To test the trained model, we need to use it to *score* the validation dataset we held back when we split the original data - in other words, predict labels for the features in the validation dataset. Expand the **Model Scoring & Evaluation** section and drag a **Score Model** module to the canvas, below the **Train Model** module. Then connect the output of the **Train Model** module to the **Trained model** (left) input of the **Score Model** module; and drag the **Results dataset2** (right) output of the **Split Data** module to the **Dataset** (right) input of the **Score Model** module.
8. Ensure your pipeline looks like this image:
    
    ![https://docs.microsoft.com/en-us/learn/wwl-data-ai/create-regression-model-azure-machine-learning-designer/media/train-score.png](https://docs.microsoft.com/en-us/learn/wwl-data-ai/create-regression-model-azure-machine-learning-designer/media/train-score.png)
    

## **Run the training pipeline**

Now you're ready to run the training pipeline and train the model.

1. Select **Submit**, and run the pipeline using the existing experiment named **mslearn-auto-training**.
2. Wait for the experiment run to complete. This may take 5 minutes or more.
3. When the experiment run has completed, select the **Score Model** module and in the settings pane, on the **Outputs + logs** tab, under **Data outputs** in the **Scored dataset** section, use the **Preview Data** icon to view the results.
4. Scroll to the right, and note that next to the **price** column (which contains the known true values of the label) there is a new column named **Scored labels**, which contains the predicted label values.
5. Close the **Score Model result visualization** window.

The model is predicting values for the **price** label, but how reliable are its predictions? To assess that, you need to evaluate the model.

# **Evaluate a regression model**

To evaluate a regression model, you could simply compare the predicted labels to the actual labels in the validation dataset to held back during training, but this is an imprecise process and doesn't provide a simple metric that you can use to compare the performance of multiple models.

## **Add an Evaluate Model module**

1. Open the **Auto Price Training** pipeline you created in the previous unit if it's not already open.
2. In the pane on the left, in the **Model Scoring & Evaluation** section, drag an **Evaluate Model** module to the canvas, under the **Score Model** module, and connect the output of the **Score Model** module to the **Scored dataset** (left) input of the **Evaluate Model** module.
3. Ensure your pipeline looks like this:
    
    ![https://docs.microsoft.com/en-us/learn/wwl-data-ai/create-regression-model-azure-machine-learning-designer/media/evaluate.png](https://docs.microsoft.com/en-us/learn/wwl-data-ai/create-regression-model-azure-machine-learning-designer/media/evaluate.png)
    
4. Select **Submit**, and run the pipeline using the existing experiment named **mslearn-auto-training**.
5. Wait for the experiment run to complete.
6. When the experiment run has completed, select the **Evaluate Model** module and in the settings pane, on the **Outputs + logs** tab, under **Data outputs** in the **Evaluation results** section, use the **Preview Data** icon to view the results. These include the following regression performance metrics:
    - **Mean Absolute Error (MAE)**: The average difference between predicted values and true values. This value is based on the same units as the label, in this case dollars. The lower this value is, the better the model is predicting.
    - **Root Mean Squared Error (RMSE)**: The square root of the mean squared difference between predicted and true values. The result is a metric based on the same unit as the label (dollars). When compared to the MAE (above), a larger difference indicates greater variance in the individual errors (for example, with some errors being very small, while others are large).
    - **Relative Squared Error (RSE)**: A relative metric between 0 and 1 based on the square of the differences between predicted and true values. The closer to 0 this metric is, the better the model is performing. Because this metric is relative, it can be used to compare models where the labels are in different units.
    - **Relative Absolute Error (RAE)**: A relative metric between 0 and 1 based on the absolute differences between predicted and true values. The closer to 0 this metric is, the better the model is performing. Like RSE, this metric can be used to compare models where the labels are in different units.
    - **Coefficient of Determination (R2)**: This metric is more commonly referred to as *R-Squared*, and summarizes how much of the variance between predicted and true values is explained by the model. The closer to 1 this value is, the better the model is performing.
7. Close the **Evaluate Model result visualization** window.

You can try a different regression algorithm and compare the results by connecting the same outputs from the **Split Data** module to a second **Train model** module (with a different algorithm) and a second **Score Model** module; and then connecting the outputs of both **Score Model** modules to the same **Evaluate Model** module for a side-by-side comparison.

When you've identified a model with evaluation metrics that meet your needs, you can prepare to use that model with new data.

# **Create an inference pipeline**

After creating and running a pipeline to train the model, you need a second pipeline known as an inference pipeline. The inference pipeline performs the same data transformations as the first pipeline for *new* data. Then it uses the trained model to *infer*, or predict, label values based on its features. This model will form the basis for a predictive service that you can publish for applications to use.

## **Create and run an inference pipeline**

1. In Azure Machine Learning Studio, expand the left-hand pane by selecting the three lines at the top left of the screen. Click on **Jobs** (under **Assets**) to view all of the jobs you have run. Select the experiment **mslearn-auto-training**, then select the **mslearn-auto-training** pipeline.
2. Locate the menu above the canvas and click on **Create inference pipeline**. You may need to expand your screen to full and click on the three dots icon **...** on the top right hand corner of the screen in order to find **Create inference pipeline** in the menu.
    
    ![https://docs.microsoft.com/en-us/learn/wwl-data-ai/create-regression-model-azure-machine-learning-designer/media/create-inference-pipeline.png](https://docs.microsoft.com/en-us/learn/wwl-data-ai/create-regression-model-azure-machine-learning-designer/media/create-inference-pipeline.png)
    
3. In the **Create inference pipeline** drop-down list, click **Real-time inference pipeline**. After a few seconds, a new version of your pipeline named **Auto Price Training-real time inference** will be opened.
    
    *If the pipeline doesn't include **Web Service Input** and **Web Service Output** modules, go back to the **Designer** page and then reopen the **Auto Price Training-real time inference** pipeline.*
    
4. Rename the new pipeline to **Predict Auto Price**, and then review the new pipeline. It contains a web service input for new data to be submitted, and a web service output to return results. Some of the transformations and training steps are a part of this pipeline. The trained model will be used to score the new data.
    
    You're going to make the following changes to the inference pipeline in the next steps #5-9:
    
    ![https://docs.microsoft.com/en-us/learn/wwl-data-ai/create-regression-model-azure-machine-learning-designer/media/inference-changes.png](https://docs.microsoft.com/en-us/learn/wwl-data-ai/create-regression-model-azure-machine-learning-designer/media/inference-changes.png)
    
    Use the image for reference as you modify the pipeline in the next steps.
    
5. The inference pipeline assumes that new data will match the schema of the original training data, so the **Automobile price data (Raw)** dataset from the training pipeline is included. However, this input data includes the **price** label that the model predicts, which is unintuitive to include in new car data for which a price prediction hasn't yet been made. Delete this module and replace it with an **Enter Data Manually** module from the **Data Input and Output** section, containing the following CSV data, which includes feature values without labels for three cars (copy and paste the entire block of text):
    
    CSVCopy
    
    ```
    symboling,normalized-losses,make,fuel-type,aspiration,num-of-doors,body-style,drive-wheels,engine-location,wheel-base,length,width,height,curb-weight,engine-type,num-of-cylinders,engine-size,fuel-system,bore,stroke,compression-ratio,horsepower,peak-rpm,city-mpg,highway-mpg
    3,NaN,alfa-romero,gas,std,two,convertible,rwd,front,88.6,168.8,64.1,48.8,2548,dohc,four,130,mpfi,3.47,2.68,9,111,5000,21,27
    3,NaN,alfa-romero,gas,std,two,convertible,rwd,front,88.6,168.8,64.1,48.8,2548,dohc,four,130,mpfi,3.47,2.68,9,111,5000,21,27
    1,NaN,alfa-romero,gas,std,two,hatchback,rwd,front,94.5,171.2,65.5,52.4,2823,ohcv,six,152,mpfi,2.68,3.47,9,154,5000,19,26
    
    ```
    
6. Connect the new **Enter Data Manually** module to the same **dataset** input of the **Select Columns in Dataset** module as the **Web Service Input**.
7. Now that you've changed the schema of the incoming data to exclude the **price** field, you need to remove any explicit uses of this field in the remaining modules. Select the **Select Columns in Dataset** module and then in the settings pane, edit the columns to remove the **price** field.
8. The inference pipeline includes the **Evaluate Model** module, which isn't useful when predicting from new data, so delete this module.
9. The output from the **Score Model** module includes all of the input features and the predicted label. To modify the output to include only the prediction:
    - Delete the connection between the **Score Model** module and the **Web Service Output**.
    - Add an **Execute Python Script** module from the **Python Language** section, replacing all of the default python script with the following code (which selects only the **Scored Labels** column and renames it to **predicted_price**):
        
        PythonCopy
        
        ```
        import pandas as pd
        
        def azureml_main(dataframe1 = None, dataframe2 = None):
        
            scored_results = dataframe1[['Scored Labels']]
            scored_results.rename(columns={'Scored Labels':'predicted_price'},
                                inplace=True)
            return scored_results
        
        ```
        
    - Connect the output from the **Score Model** module to the **Dataset1** (left-most) input of the **Execute Python Script**, and connect the output of the **Execute Python Script** module to the **Web Service Output**.
10. Verify that your pipeline looks similar to the following image:
    
    ![https://docs.microsoft.com/en-us/learn/wwl-data-ai/create-regression-model-azure-machine-learning-designer/media/inference-pipeline.png](https://docs.microsoft.com/en-us/learn/wwl-data-ai/create-regression-model-azure-machine-learning-designer/media/inference-pipeline.png)
    
11. Submit the pipeline as a new experiment named **mslearn-auto-inference** on your compute cluster. The experiment may take a while to run.
12. When the pipeline has completed, select the **Execute Python Script** module, and in the settings pane, on the **Output + logs** tab, visualize the **Result dataset** to see the predicted prices for the three cars in the input data.
13. Close the visualization window.

Your inference pipeline predicts prices for cars based on their features. Now you're ready to publish the pipeline so that client applications can use it.

# **Deploy a predictive service**

After you've created and tested an inference pipeline for real-time inferencing, you can publish it as a service for client applications to use.

**Note**

In this exercise, you'll deploy the web service to an Azure Container Instance (ACI). This type of compute is created dynamically, and is useful for development and testing. For production, you should create an *inference cluster*, which provides an Azure Kubernetes Service (AKS) cluster that provides better scalability and security.

## **Deploy a service**

> 
> 
1. View the **Predict Auto Price** inference pipeline you created in the previous unit.
2. At the top right, select **Deploy**, and deploy a new real-time endpoint, using the following settings:
    - **Name**: predict-auto-price
    - **Description**: Auto price regression.
    - **Compute type**: Azure Container Instance
3. Wait for the web service to be deployed - this can take several minutes. The deployment status is shown at the top left of the designer interface.

## **Test the service**

1. On the **Endpoints** page, open the **predict-auto-price** real-time endpoint.
2. When the **predict-auto-price** endpoint opens, view the **Consume** tab and note the following information there. You need this to connect to your deployed service from a client application.
    - The REST endpoint for your service
    - The Primary Key for your service
3. Observe that you can use the ⧉ link next to these values to copy them to the clipboard.
4. With the **Consume** page for the **predict-auto-price** service page open in your browser, open a new browser tab and open a second instance of [Azure Machine Learning studio](https://ml.azure.com/). Then in the new tab, view the **Notebooks** page (under **Author**).
5. Navigate to the left-hand pane and click on **Notebooks**. Then use the **🗋** button to create a new file with the following settings:
    - **File location**: Users/*your user name*
    - **File name**: Test-Autos.ipynb
    - **File type**: Notebook
    - **Overwrite if already exists**: Selected

> 
> 
1. When the new notebook has been created, ensure that the compute instance you created previously is selected in the **Compute** box, and that it has a status of **Running**.
2. Use the **≪** button to collapse the file explorer pane and give you more room to focus on the **Test-Autos.ipynb** notebook tab.
3. In the rectangular cell that has been created in the notebook, paste the following code:
    
    PythonCopy
    
    ```
    endpoint = 'YOUR_ENDPOINT' #Replace with your endpoint
    key = 'YOUR_KEY' #Replace with your key
    
    import urllib.request
    import json
    import os
    
    # Prepare the input data
    data = {
        "Inputs": {
            "WebServiceInput0":
            [
                {
                        'symboling': 3,
                        'normalized-losses': None,
                        'make': "alfa-romero",
                        'fuel-type': "gas",
                        'aspiration': "std",
                        'num-of-doors': "two",
                        'body-style': "convertible",
                        'drive-wheels': "rwd",
                        'engine-location': "front",
                        'wheel-base': 88.6,
                        'length': 168.8,
                        'width': 64.1,
                        'height': 48.8,
                        'curb-weight': 2548,
                        'engine-type': "dohc",
                        'num-of-cylinders': "four",
                        'engine-size': 130,
                        'fuel-system': "mpfi",
                        'bore': 3.47,
                        'stroke': 2.68,
                        'compression-ratio': 9,
                        'horsepower': 111,
                        'peak-rpm': 5000,
                        'city-mpg': 21,
                        'highway-mpg': 27
                },
            ],
        },
        "GlobalParameters":  {
        }
    }
    body = str.encode(json.dumps(data))
    headers = {'Content-Type':'application/json', 'Authorization':('Bearer '+ key)}
    req = urllib.request.Request(endpoint, body, headers)
    
    try:
        response = urllib.request.urlopen(req)
        result = response.read()
        json_result = json.loads(result)
        y = json_result["Results"]["WebServiceOutput0"][0]
        print(y)
    
    except urllib.error.HTTPError as error:
        print("The request failed with status code: " + str(error.code))
    
        # Print the headers to help debug the error
        print(error.info())
        print(json.loads(error.read().decode("utf8", 'ignore')))
    
    ```
    
    **Note**
    
    Don't worry too much about the details of the code. It just submits details of a car and uses the **predict-auto-price** service you created to get a predicted price.
    
4. Switch to the browser tab containing the **Consume** page for the **predict-auto-price** service, and copy the REST endpoint for your service. Then switch back to the tab containing the notebook and paste the key into the code, replacing YOUR_ENDPOINT.
5. Switch to the browser tab containing the **Consume** page for the **predict-auto-price** service, and copy the Primary Key for your service. Then switch back to the tab containing the notebook and paste the key into the code, replacing YOUR_KEY.
6. Save the notebook. Then use the **▷** button next to the cell to run the code.
7. After you have run the code, scroll down to the bottom of the screen. You should see the output **'predicted_price'**. The output is the predicted price for a vehicle with the particular input features specified in the code.