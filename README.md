# okdir

## 安装

```shell
pip install okdir
```

## 使用

以下以我们需要建立一个物种描述文件夹为例来说明okdir的使用，该文件夹的结构为：

```
.
├── metadata.json         // 物种元数据，包含物种名、异名、分类等
├── thumbnail.png         // 物种缩略图
├── morphology.txt        // 物种描述
├── synonyms.txt          // 物种异名
├── taxons.txt            // 物种分类信息
├── pictures              // 物种图片文件夹
│   ├── lossless    // 无损图片文件夹
│   └── thumbnails  // 缩略图文件夹
├── sequences             // 序列文件夹
├── references.txt        // 参考文献
├── distributions.txt     // 分布地文件
└── collections.txt       // 采集地文件
```

### 定义文件夹

```python
from okdir import File, Dir


class PicturesDir(Dir):
    lossless_dir = Dir('lossless')
    thumbnails_dir = Dir('thumbnails')


class SpeciesDir(Dir):
    metadata_file = File('metadata.json')
    thumbnail_file = File('thumbnail.png')
    morphology_file = File('morphology.txt')
    synonyms_file = File('synonyms.txt')
    taxons_file = File('taxons.txt')
    pictures_dir = PicturesDir('pictures')
    sequences_dir = Dir('sequences')
    references_file = File('references.txt')
    distributions_file = File('distributions.txt')
    collections_file = File('collections.txt')
```

### 使用文件夹

**1 实例化SpeciesDir**

```python
species_dir = SpeciesDir('/Users/dev/Tests/24120901', True)
```

（1）第一个参数：`/Users/dev/Tests/24120901`代表`SpeciesDir`根目录
（2）第二个参数`True`代表将此根目录应用到其所有的子目录、子文件中

**2 检查子目录、子文件是否存在**

```python
try:
    species_dir.check()
except FileNotFoundError:
    print('有子目录或子文件不存在')
```

**3 创建子目录**

```python
species_dir.mkdir()
```

**4 创建子文件**

```python
species_dir.touch()
```

**5 获取文件夹路径**

任何File、Dir的实例都有一个path属性，为一个pathlib.Path的实例：

```python
print(species_dir.pictures_dir.lossless_dir.path)
print(species_dir.pictures_dir.lossless_dir.path / 'foo.png')
```