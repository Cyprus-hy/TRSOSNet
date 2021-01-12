# TRSOSNet

A novel pedestrian re-identification network called TRSOSNet, based on OSNet, rerank and channel shuffle.

## Installation

At first, I recommend you install [anaconda](https://docs.anaconda.com/anaconda/install/). And then type the following commands to install the dependencies that TRSOSNet requires.

```bash
# clone this repo
git clone https://github.com/Cyprus-hy/TRSOSNet

# create environment
cd TRSOSNet
conda create --name TRSOSNet python=3.7
conda activate TRSOSNet

# install dependencies
pip install -r requirements.txt

# install torch and torchvision (select the proper cuda version to suit your machine)
conda install pytorch torchvision cudatoolkit=10.1 -c pytorch

# install torchreid (don't need to re-build it if you modify the source code)
python setup.py develop
```

## Datasets

In TRSOSNet, I use two datasets to train and test. One is [market1501](https://drive.google.com/file/d/0B8-rUzbwVRk0c054eEozWG9COHM/view?usp=sharing), the other is [dukemtmcreid](https://drive.google.com/open?id=1jjE85dRCMOgRtvJ5RQV9-Afs-2_5dY3O). You can create a folder first, and then put the two datasets in it. Like: `mkdir data`. And then the category should like this:

```
data
----dukemtmc-reid
----market1501
```

## Train

You can train TRSOSNet from scratch.

```bash
# train TRSOSNet on market1501 from scratch
python scripts/main.py \
--config-file configs/trsosnet_market1501.yaml \
data.root path_to_your_data \
data.save_dir path_to_save_your_model

# train TRSOSNet on dukemtmcreid from scratch
python scripts/main.py \
--config-file configs/trsosnet_dukemtmcreid.yaml \
data.root path_to_your_data \
data.save_dir path_to_save_your_model
```

Or you can directly use pretrained model. You can get the pretrained model of TRSOSNet and OSNet on market1501 and dukemtmcreid [here](https://drive.google.com/drive/folders/13e1xZuplwVUzvLsS3HQ1wUJ86HEXqs5W?usp=sharing).

## Test

You can use the following commands for test.

```bash
# verify the performance of TRSOSNet on market1501
python scripts/main.py \
--config-file configs/trsosnet_market1501.yaml \
model.load_weights path_to_your_pretrained_model \
test.evaluate True

# verify the performance of TRSOSNet on dukemtmcreid
python scripts/main.py \
--config-file configs/trsosnet_dukemtmcreid.yaml \
model.load_weights path_to_your_pretrained_model \
test.evaluate True

# compare the performance of OSNet on market1501 with TRSOSNet
python scripts/main.py \
--config-file configs/osnet_market1501.yaml \
model.load_weights path_to_your_pretrained_model \
test.evaluate True

# compare the performance of OSNet on dukemtmcreid with TRSOSNet
python scripts/main.py \
--config-file configs/osnet_dukemtmcreid.yaml \
model.load_weights path_to_your_pretrained_model \
test.evaluate True
```

If you want to visualize the results, you can add `test.visrank True` at the end of each command. And the visualizations will be saved in folder `log` defaultly.

## Quantitive Comparison

![](https://ftp.bmp.ovh/imgs/2021/01/54db87266fb75cd6.png)

## Qualitative Comparison

OSNet on market1501:

![OSNet on market1501](https://ftp.bmp.ovh/imgs/2021/01/5767bafb80ca827d.jpg)

TRSOSNet on market1501:

![](https://ftp.bmp.ovh/imgs/2021/01/9723449ca5567b02.jpg)

OSNet on dukemtmcreid:

![](https://ftp.bmp.ovh/imgs/2021/01/01370ef74380960f.jpg)

TRSOSNet on dukemtmcreid:

![](https://ftp.bmp.ovh/imgs/2021/01/4538fb224570854c.jpg)

## References

> [1] Zhou K, Yang Y, Cavallaro A, et al. Omni-scale feature learning for person re-identification[C]//Proceedings of the IEEE International Conference on Computer Vision. 2019: 3702-3712.
>
> [2] Zhong Z, Zheng L, Cao D, et al. Re-ranking person re-identification with k-reciprocal encoding[C]//Proceedings of the IEEE Conference on Computer Vision and Pattern Recognition. 2017: 1318-1327.
>
> [3] Zhang X, Zhou X, Lin M, et al. Shufflenet: An extremely efficient convolutional neural network for mobile devices[C]//Proceedings of the IEEE conference on computer vision and pattern recognition. 2018: 6848-6856.
