# cancer-detection
Contest Link: https://www.kaggle.com/c/syde522/overview
Dataset Info: https://www.nature.com/articles/srep27988
Dataset Summary:
- This dataset contains segments of 10 Colorectal cancer tissue slides obtained from the pathology archive at the University Medical Center Mannheim
- These slides were then divided into 150x150 images in multiple different classes (from the link above):
    - `01_TUMOR` - Tumour epithelium
    - `02_STROMA` - Simple stroma (homogeneous composition, includes tumour stroma, extra-tumoural stroma and smooth muscle);
    - `03_COMPLEX` - Complex stroma (containing single tumour cells and/or few immune cells)
    - `04_LYMPHO` - Immune cells (including immune-cell conglomerates and sub-mucosal lymphoid follicles)
    - `05_DEBRIS` - Debris (including necrosis, hemorrhage and mucus)
    - `06_MUCOSA` - Normal mucosal glands
    - `07_ADIPOSE` - Adipose tissue
    - `08_EMPTY` - Background (no tissue)

Best performing Solution:
- Create 5 splits of the data
- Fine-tune a VGG16 network on each split
    - Training takes 3 steps.  For each step, unfreeze a few more layers of the network
    - Part of the logic here was also that every time training is restarted, the optimizer is reset, possibly bumping the network out of some local minimmums
- Average predictions of each VGG16 network for final submission
- Results: 96.4% test set accuracy

File List:
- `create_datasets.ipynb` create 5 train/test combinations using sklearn's KFold
- `vgg16_retraining.ipynb` fine-tuning of a single VGG16 network using one fold of data
- `create_ensemble.ipynb` create ensemble network of multiple VGG16 models and use them to make predictions (this one is a bit messy)

Ideas for the Future (Partially from [here](https://www.kaggle.com/havenlau/97-w-effnetb4-radam-noisy-student-swish-act))
- Incorporate Test time Augmentation to further improve model consistency
- Create a specific data augmentation function to simulate different concentrations of dye in the images from microscope slides
- Look into use of RAdam/Ranger for a more robust convergence: https://github.com/LiyuanLucasLiu/RAdam, https://github.com/lessw2020/Ranger-Deep-Learning-Optimizer
- Try fine-tuning Efficientnet pretrained with noisy-student method for better performance