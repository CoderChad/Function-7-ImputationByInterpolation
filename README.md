# Function-7-ImputationByInterpolation

Specific things this function will do:


a) Take in one input argument, a dataset that represents a time series. Importantly, this dataset might contain missing data.


b) The function should replace any missing data by linear interpolation.


c) The function should return the dataset with interpolated values computed in b)


d) Assumptions: Input argument should be a one-dimensional array (by default a numpy array) or arbitrary length


e) Make sure the function has a clear header as to what inputs the function assumes, what outputs it produces and when it
was written.


import numpy as np

def imputationByInterpolation(data):
    
    nans01 = np.isnan(data).astype(int)
    nans = np.where(nans01==1)[0]
    
    # this line splits the data into groups of consecutive numbers
    groups = np.split(nans, np.where(np.diff(nans) != 1)[0]+1)

    for g in groups:

        # if there are consecutive missing values, use linspace
        if len(g) > 1:
            i_beg = g[0]-1
            i_end = g[-1]+1
            interpolation = np.linspace(data[i_beg],data[i_end],len(g)+2)
            new_vals = interpolation[1:-1]

            for i,j in enumerate(g):
                data[j] = new_vals[i]

        # if there is one missing value by itself, take average of adjacent numbers
        elif len(g) == 1:
            data[g[0]] = (data[g[0]-1]+data[g[0]+1])/2
        
    return data
data=np.genfromtxt('./data/sampleimput1.csv')
newdata=imputationByInterpolation(data)
