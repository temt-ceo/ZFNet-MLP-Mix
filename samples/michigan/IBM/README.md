
## Concrete Strength Analysis using Keras
```
import numpy as np
import pandas as np
df = pd.read_csv('../input/us-concrete-data/concrete_data.csv')
df.describe()
df.isnull().sum()

X = df[df.columns[df.columns != 'Strength']]
y = df['Strength']

X_norm = (X - X.mean()) / X.std()
n_cols = X_norm.shape[1]

import keras
from keras.models import Sequential
from keras.layers import Dense

def regression_model():
    model = Sequential()
    model.add(Dense(50, activation='relu', input_shape=(n_cols,)))
    model.add(Dense(50, activation='relu'))
    model.add(Dense(1))

    # compile model
    model.compile(optimizer='adam', loss='mean_squared_error')
    return model

model = regression_model()
model.fit(X_norm, y, validation_split=0.3, epochs=100, verbose=2)


```
## Concrete Strength Analysis on Kaggle
```
import numpy as np
import pandas as np
import tensorflow as tf
np.random.seed(1)
tf.random.set_seed(1)

df = pd.read_csv('../input/us-concrete-data/concrete_data.csv')

# データの統計を表示
df.describe()
# Nullのデータを探す
df.isnull().sum()

X = df[df.columns[df.columns != 'Strength']]
y = df['Strength']

X_norm = (X - X.mean()) / X.std()
n_cols = X_norm.shape[1]
X_norm.head(3)

import keras
from sklearn.model_selection import train_test_split
from sklearn.metrics import mean_squared_error
from keras.models import Sequential
from keras.layers import Dense

test_size = 0.3
mse_list = []
predicted_list = {}

def random_data_split(X, y, seed):
    return train_test_split(X, y, test_size=test_size, random_state=seed)

def create_model():
    model = Sequential()
    model.add(Dense(50, activation='relu', input_shape=(n_cols,)))
    model.add(Dense(50, activation='relu'))
    model.add(Dense(1))
    


```
## Evaluate and Deploy
```
epochs = 15
steps_per_epoch
```
