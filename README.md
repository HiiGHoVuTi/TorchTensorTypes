
# Torch Tensor Types

This package is a Quality of Life improvement when prototyping and processing Tensor objects from the pyTorch library.
The TensorType class is a Pipeline for preprocessing tensors automatically, and include multiple utility methods. `my_TensorType<<myData`
You can add TensorTypes together to have a longer preprocessing pipeline. `myTensorType + myOtherTensorType`

For example, this code 
```py
fake_image = model(torch.unsqueeze(real_image, 0).cuda()).cpu().detach().numpy()[0]
```
can be replaced by
```py
fake_image = SingleDisplayableImage<<model(ModelInputFormat<<real_image)
```

### The list of arguments to the constructor is:
```py
TensorType:
__init__(shape=None, transforms=[], torch_methods=[],
to_batch=False, device=None, from_single_value=False,
random_values=False, to_numpy=False, detach=False):
```
All of these should be pretty telling by their name, if you know pyTorch.
- `TensorType.shape`: the input will be viewed as this shape
- `TensorType.transforms`: a list of functions that will be applied at the end
- `TensorType.torch_methods`: a list of method names that will be called on the tensor i.e `"mean" => tensor.mean()`
- `TorchType.to_batch`: will unsqueeze the data into a batch with a single sample
- `TorchType.device`: transfers the tensor to a device
- `TorchType.from_single_value`: creates a uniform tensor from a single value
- `TorchType.random_values`: creates a tensor from `torch.rand`
- `TensorType.to_numpy`: outputs a numpy array
- `TensorType.detach`: detachs the tensor from the graph

### Syntax
```py
from TTT import TensorType as TT
# Creates a uniform image tensor pipeline
myTensorType = TT(shape=(3, 224, 224), from_single_value=True)
# A black image
data = myTensorType<<0
# "myTensorType" will parse first, then the other TT
myDisplayableImage = myTensorType + TT(to_numpy=True, transforms=[np.transpose])
# A white image ready to display
myDisplayableImage<<1
```