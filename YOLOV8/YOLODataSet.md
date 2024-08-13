# YOLODataset

- the parent of the class `YOLODataset` is `BaseDataset`
  
  - `YOLODataset` is defined in `ultralytics/data/dataset.py`
  
  - `BaseDataset` is defined in `ultralytics/data/base.py`

- `BaseDataset` arguments
  
  - img_path (str): Path to the folder containing images
  
  - imgsz (int, optional): Image size. Defaults to 640.
  
  - cache (bool, optional): Cache images to RAM or disk during training. Defaults to False.
  
  - augment (bool, optional): If True, data augmentation is applied. Defaults to True.    
  
  - hyp (dict, optional): Hyperparameters to apply data augmentation. Defaults to None.
  
  - prefix (str, optional): Prefix to print in log messages. Defaults to ''.
  
  - rect (bool, optional): If True, rectangular training is used. Defaults to False.
  
  - batch_size (int, optional): Size of batches. Defaults to None.
  
  - stride (int, optional): Stride. Defaults to 32.
  
  - pad (float, optional): Padding. Defaults to 0.0.
  
  - single_cls (bool, optional): If True, single class training is used. Defaults to False.
  
  - classes (list): List of included classes. Default is None.
  
  - fraction (float): Fraction of dataset to utilize. Default is 1.0 (use all data).

# 相关函数`ultralytics/cfg/xxx`

## 1 get_cfg

it can return an object `ultralytics.utils.IterableSimpleNamespace`

```python
from ultralytics.cfg import get_cfg
# Load default configuration
config = get_cfg()
# Load from a custom file with overrides
config = get_cfg('path/to/config.yaml', overrides={'epochs': 50, 'batch_size': 16})
```

## 2  check_dict_alignment

检查自定义和基本配置字典之间的键对齐情况，处理已弃用的键，并为不匹配的键提供信息性错误消息。

Args:

- base (dict): The base configuration dictionary containing valid keys.

- custom (dict): The custom configuration dictionary to be checked for alignment.

- e (Exception, optional): An optional error instance passed by the calling function. Default is None.

```python
base_cfg = {'epochs': 50, 'lr0': 0.01, 'batch_size': 16}
custom_cfg = {'epoch': 100, 'lr': 0.02, 'batch_size': 32}

        try:
            check_dict_alignment(base_cfg, custom_cfg)
        except SystemExit:
            # Handle the error or correct the configuration
```

## 3 cfg2dict

Convert a configuration object to a dictionary, whether it is a file path, a string, or a SimpleNamespace object.

    Args:
        cfg (str | Path | dict | SimpleNamespace): Configuration object to be converted to a dictionary. This may be a
            path to a configuration file, a dictionary, or a SimpleNamespace object.
    
    Returns:
        (dict): Configuration object in dictionary format.

- Example:

```python
from ultralytics.cfg import cfg2dict
from types import SimpleNamespace

# Example usage with a file path
config_dict = cfg2dict('config.yaml')

# Example usage with a SimpleNamespace
config_sn = SimpleNamespace(param1='value1', param2='value2')
config_dict = cfg2dict(config_sn)

# Example usage with a dictionary (returns the same dictionary)
config_dict = cfg2dict({'param1': 'value1', 'param2': 'value2'})
```

Notes:

- If `cfg` is a path or a string, it will be loaded as YAML and converted to a dictionary.

- If `cfg` is a SimpleNamespace object, it will be converted to a dictionary using `vars()`

## 4 select_device

```python
from ultralytics.utils.torch_utils import select_device
```

> Args:
>         device (str | torch.device, optional): Device string or torch.device object.
>             Options are 'None', 'cpu', or 'cuda', or '0' or '0,1,2,3'. Defaults to an empty string, which auto-selects
>             the first available GPU, or CPU if no GPU is available.
>         batch (int, optional): Batch size being used in your model. Defaults to 0.
>         newline (bool, optional): If True, adds a newline at the end of the log string. Defaults to False.
>         verbose (bool, optional): If True, logs the device information. Defaults to True.
> 
> Returns:
>  (torch.device): Selected device.
> 
> Raises:
>  ValueError: If the specified device is not available or if the batch size is not a multiple of the number of
>  devices when using multiple GPUs.
> 
> Examples:
>         >>> select_device('cuda:0')
>         device(type='cuda', index=0)
> 
> ```
>     >>> select_device('cpu')
>     device(type='cpu')
> ```

如果`device`是一个空值，函数会自动输出当前yolo和Python的版本,并自动匹配设备，优先选择GPU.

如果不想输出这类提示，需要直接传`troch.devcie`对象给该函数

```python
import torch
dev = torch.device(type='cpu')
device = select_device(dev)
```

# 构建YOLODataset, Trainer对象

- 获取官方提供的参数配置文件

```python
from ultralytics.cfg import get_cfg
cfg = get_cfg()
```

```python
import ultralytics
cfg = ultralytics.utils.DEFAULT_CFG
```

- 默认条件下，`cfg.data`是空值，YOLO需要根据该值区创建`Dataset`和`DetectionTrainer`对象

- `cfg.data`是一个`yaml`文件，提供详细的数据和分类信息，如下：

```yaml
names:
- buffalo
- deer
- elephant
- rhino
path: /home/hxl170008/code/NSF/paper_redundancy/data
train: yolo_train.txt
val: yolo_val.txt
```

`yolo_train.txt, yolo_val.txt` 存储再`path`对应的目录下。`yolo_train.txt, yolo_val.txt`记录每个图片的绝对路径, 该`.yaml`文件被存放在我的服务器，路径为`data/yaml/bedr.yaml`

- 更新`cfg.device`, `cfg.data`,然后基于`cfg`去创建`DetectionTrainer`对象

```python
import ultralytics, pathlib
from ultralytics.utils.torch_utils import select_device
from ultralytics.cfg import get_cfg
from ultralytics.models.yolo.detect import DetectionTrainer

cfg = ultralytics.utils.DEFAULT_CFG
# update cfg.device
dev = torch.device('cpu')
cfg = get_cfg(cfg=cfg, overrides={'device':dev})
# update cfg.data
yaml_path = 'data/yaml/bedr.yaml'
cfg = get_cfg(cfg=cfg, overrides={'data':yaml_path})
# 设置训练数据输出保存路径,路径必须是`pathlib.Path`对象
cfg.save_dir = pathlib.Path("backup/YOLO_BDER/v8/n/train01")
# Create DetectionTrainer object
trainer = DetectionTrainer(cfg=cfg)
```

- 通过trainer对象可以获得训练集和validation dataset的地址

```python
# get the path of train_dataset and validation dataset by trainer
train_img_path, val_img_path =trainer.get_dataset()
```

`('/home/hxl170008/code/NSF/paper_redundancy/data/yolo_train.txt',
 '/home/hxl170008/code/NSF/paper_redundancy/data/yolo_val.txt')`

- 调用类`DetectionTrainer`的内置属性函数`build_dataset`构建数据集
  
  - `build_datast`函数有三个参数：`img_path`, `mode`, `batch`
  
  - 当`mode='train'`时候，YOLO会以用一系列Augment函数方法去对训练集进行变换， 当采用`rect`即`cfg.rect=True`时候，需要根据`batch`的值建立缓存。如果`batch`是`None`, 构建训练数据集会报错
  
  - 当`mode='test',构建验证集，不需要对图片进行变换，``batch`值存在与否都不可以
  
  - 总之，最简单的方法不出差错是，构建训练集和验证集都提供具体的`batch`.

```python
train_img_path, val_img_path = trainer.get_dataset()
# create training dataset
ds_train = trainer.build_dataset(img_path=train_img_path, mode='train', batch=cfg.batch)
# create validation dataset,
ds_val = trainer.build_dataset(img_path=val_img_path,mode='test')
```

- 调用类`DetectionTrainer`的内置属性函数`get_dataloader`构建`DataLoader`对象
  
  - `get_dataloader`的参数
    
    - dataset_path:.txt文件，每行记录一个图片的路径
    
    - batch_size: 
    
    - rank: 默认值0，但是如果我们只有一个GPU或没有GPU,rank设置为-1
    
    - mode: 只有两个值：`train`, `val`

```python
train_img_path, val_img_path = trainer.get_dataset()
# create training dataloader
dloader_train = trainer.get_dataloader(dataset_path=train_img_path,batch_size=8, rank=-1,mode='train')
# create validation dataloader
dloader_val  = trainer.get_dataloader(dataset_path=val_img_path, batch_size=4,rank=-1, mode='val')
```

# 训练模型

在前面的基础上，已经构建了`DetectionTrainer`对象

- 设置训练的参数
  
  - `cfg.save_dir`: 训练输出结果保存的路径
  
  - `cfg.workers`: cpu内核使用个数
  
  - `cfg.batch`: 
  
  - `cfg.epochs`: the number of training epoches
  
  - `cfg.patience=30`: the value of patience，一种退出机制

根据需要设置完成后，直接调用函数`train()`开始训练

```python
trainer.train()
```

# 构建DetectionValidator

`DetectionValidator` has three arguments:

- dataloader

- save_dir: 必须是`pathlib.Path`对象

- pbar: default value None

- _callbacks: _

```python
from ultralytics.models.yolo.detect import DetectionValidator
cfg.save_dir = pathlib.Path("backup/YOLO_BDER/v8/n/train01")
validator = DetectionValidator(dataloader=dloader_val,save_dir=cfg.save_dir)
```

# 模型预测

1. 根据模型权重文件,like `weights/best.pt`创建一个`YOLO`model.

```python
from ultralytics import YOLO
p = r"backup/YOLO_BDER/v8/n/train01/weights/best.pt"
model = YOLO(model=p,task='detect')
```

2. 模型成功初始化后，将生成一个`overrides`变量，用于将来更新系统配置参数`cfg`.

```python
model.overrides
//
{'task': 'detect',
 'data': 'data/yaml/bedr.yaml',
 'imgsz': 640,
 'single_cls': False,
 'model': 'backup/YOLO_BDER/v8/n/train01/weights/best.pt'}
```

3. 调用`YOLO model`的`predict`函数，再该函数内包含自定义的参数配置`custom`, 它和`overrides` 共同构成一个字典`args` ,用于更新模型配置参数

```python
custom = {"conf": 0.25, "batch": 1, "save": is_cli, "mode": "predict"} 
args = {**self.overrides, **custom, **kwargs}
```

4. 在`predict`函数里构建了一个`DetectionPredictor`对象

5. 调用`DetectionPredictor`对象的预测函数`predict_cli`

```python
source = 'data/123.jpg'
self.predictor.predict_cli(source=source)
```

6. 在`predict_cli` 继续调用函数`stream_inference`

7. `DetectionPredictor`对象的函数`inference`生成预测结果

```python
preds = self.inference(im,*args, **kwargs)
```

8. 调用函数`postprocess`将预测结果`preds`变成 `SimpleClass`对象：`self.results`

9. 接着调用函数`write_results` 生成预测效果图

10. 最终，`model.predict(xx)`返回的是一个对象：`ultralytics.engine.results.Results`, 它继承自`SimpleClass`

11. `Results`对象提供`plot`函数可以用来画效果图

# YOLO汲取的编程思想

## IterableSimpleNamespace

It inherits from the class `typing.SimpleNamespace`, which extends the class of SimpleNamespace that adds iterable functionality and enables usage with `dict()` and `for loops`.

```python
from types import SimpleNamespace

class IterableSimpleNamespace(SimpleNamespace):
    """Ultralytics IterableSimpleNamespace is an extension class of SimpleNamespace that adds iterable functionality and
    enables usage with dict() and for loops.
    """

    def __iter__(self):
        """Return an iterator of key-value pairs from the namespace's attributes."""
        return iter(vars(self).items())

    def __str__(self):
        """Return a human-readable string representation of the object."""
        return "\n".join(f"{k}={v}" for k, v in vars(self).items())

    def __getattr__(self, attr):
        """Custom attribute access error message with helpful information."""
        name = self.__class__.__name__
        raise AttributeError(
            f"""
            '{name}' object has no attribute '{attr}'. This may be caused by a modified or out of date ultralytics
            'default.yaml' file.\nPlease update your code with 'pip install -U ultralytics' and if necessary replace
            {DEFAULT_CFG_PATH} with the latest version from
            https://github.com/ultralytics/ultralytics/blob/main/ultralytics/cfg/default.yaml
            """
        )

    def get(self, key, default=None):
        """Return the value of the specified key if it exists; otherwise, return the default value."""
        return getattr(self, key, default)
```
