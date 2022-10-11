# Tensor

In PyTorch, Tensor is the important component in constructing dynamic computational graph. It contains **data** and **grad**, which storage the value of node and gradient w.r.t loss respectively($\frac {\partial loss}{\partial w}$).

## 用PyTorch实现线性模型

```python
import torch

x_data = [1.0, 2.0, 3.0]
y_data = [2.0, 4.0, 6.0]

w = torch.Tensor([1.0])
w.requires_grad = True
```

> If **autograd mechanics** are required, the element variable `requires_grad` of **Tensor** has to be set to **True**.

