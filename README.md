# README #

Flatten layer for PyTorch framework

# Get it!
=========

Install it with PyPI dyrectly from github repository
```shell
pip install git+git://github.com/levants/pytorch-flatten-layer.git
```

# Use it!
=========

```python
from nn.flatten import Flatten

class Net(nn.Module):
  """Network model with flatten layer
   for character recognition"""
  
  def __init__(self):
    super(Net, self).__init__()
    self.conv1 = nn.Conv2d(1, 10, kernel_size=5)
    self.conv2 = nn.Conv2d(10, 20, kernel_size=5)
    self.conv2_drop = nn.Dropout2d()
    self.flatten = Flatten(50)
    self.fc2 = nn.Linear(50, 10)
  
  def forward(self, x):
      
    x = F.relu(F.max_pool2d(self.conv1(x), 2))
    x = F.relu(F.max_pool2d(self.conv2_drop(self.conv2(x)), 2))
    x = self.flatten(x)
    x = F.relu(x)
    x = F.dropout(x, training=self.training)
    x = self.fc2(x)
    result = F.log_softmax(x)
    
    return result
```

For sequencial model

```python
from nn.flatten import Flatten

nn.Sequential(nn.Conv2d(1, 10, kernel_size=5),
              nn.MaxPool2d(2, 2),
              nn.ReLU(),
              nn.Conv2d(10, 20, kernel_size=5),
              nn.MaxPool2d(2, 2),
              nn.ReLU(),
              nn.Dropout2d(),
              Flatten(50),
              nn.Linear(50, 10))
```

No additional manipulation like ```python x = x.view(x.size(0), 320)``` is needed on tensors after convolutional 
layer for passing it to Flatten layer

```python
x = F.relu(F.max_pool2d(self.conv1(x), 2))
x = F.relu(F.max_pool2d(self.conv2_drop(self.conv2(x)), 2))
x = self.flatten(x)
x = F.relu(x)
```

For only vectorization of tensor better use ```python Vectorizer``` class (better initialize it once 
and use this instance in your model)

```python
from nn.flatten import Vectorizer
...
class Net(nn.Module):
  """Network model with flatten layer
   for character recognition"""
  
  def __init__(self):
    super(Net, self).__init__()
    self.vectorizer = Vectorizer
    ...

  def forward(self, x):
  ...
  x = self.flatten(x)
  ...
```
   

Enjoy :)
    


