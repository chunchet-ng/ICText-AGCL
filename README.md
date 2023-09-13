# ICText-AGCL

Official repository of the paper: "When IC Meets Text: Towards a Rich Annotated Integrated Circuit Text Dataset"

Authored By:
Chun Chet Ng*, Che-Tsung Lin*, Zhi Qin Tan, Xinyu Wang, Jie Long Kew, Chee Seng Chan, Christopher Zach

*Equal Contribution

***The manuscript is currently under review.***

[Project Page]() | [Paper]() | [ICText Dataset](#integrated-circuit-text-ictext-dataset) | [AGCL](#attribute-guided-curriculum-learning-agcl) | [Bibtex](#citation)

## Abstract:
Automated Optical Inspection (AOI) is a process that uses cameras to autonomously scan printed circuit boards for quality control. Text is often printed on chip components, and it is crucial that this text is correctly recognized during AOI, as it contains valuable information. In this paper, we introduce ICText, the largest dataset for text detection and recognition on integrated circuits. Uniquely, it includes labels for character quality attributes such as low contrast, blurry, and broken. While loss-reweighting and Curriculum Learning (CL) have been proposed to improve object detector performance by balancing positive and negative samples and gradually training the model from easy to hard samples, these methods have had limited success with one-stage object detectors commonly used in industry. To address this, we propose Attribute-Guided Curriculum Learning (AGCL), which leverages the labeled character quality attributes in ICText. Our extensive experiments demonstrate that AGCL can be applied to different detectors in a plug-and-play fashion to achieve higher Average Precision (AP), significantly outperforming existing methods on ICText without any additional computational overhead during inference. Furthermore, we show that AGCL is also effective on the generic object detection dataset Pascal VOC.
## Integrated Circuit Text (ICText) Dataset
The ViTrox-ORCR, ICText, and ICText-LT datasets can be accessed by following the steps outlined below.

### Access to Dataset
To access the dataset, you must first download and sign a [Non-Disclosure Agreement (NDA) form](https://drive.google.com/drive/folders/1o13Kej3dpMhG0NuVihXD26MD2S5NoFBJ). A sample is also provided in the shared folder. Please refer to the following section for more details. Once you have signed the NDA, email it to us at `ictext at vitrox.com`. After verifying the signed NDA, we will grant you access to download the dataset.

### Signing of NDA
You must sign the NDA electronically using Adobe Reader ([online](https://www.adobe.com/acrobat/online/sign-pdf.html) or [offline](https://helpx.adobe.com/reader/using/sign-pdfs.html)) and send us the signed NDA in pdf format. The NDA should not be in the form of pictures or scanned handwritten documents. For participants without any affiliation, you must fill in "{YOUR NAME} - Individual Participant" in the affiliation field. Please fill in the relevant details on page 2, under the line '[Representative, Affiliation, Address of LICENSEE]' and sign at the 'Signature LICENSEE' section using black ink. If you have an affiliation, rename the NDA as '{AFFILIATION}_ICTEXT_LICENSE_AGREEMENT.pdf'. Otherwise, rename it as '{YOUR_NAME}_ICTEXT_LICENSE_AGREEMENT.pdf' before emailing it to us. NDAs that are not filled in according to the stated rules will be rejected.

### ViTrox-OCR Dataset
The ViTrox-OCR dataset was introduced in the previous ICDAR 2021 Competition on Integrated Circuit Text Spotting and Aesthetic Assessment competition:

1. [Paper](https://link.springer.com/chapter/10.1007/978-3-030-86337-1_44)
2. [Competition Page](https://ictext.v-one.my/)
3. [RRC-ICText Eval GitHub Repo](https://github.com/vitrox-technologies/ictext_eval)
4. [RRC-ICText Online Evaluation Platform](https://eval.ai/web/challenges/challenge-page/756)

Please note that some annotations are outdated. Use this dataset only if you want to evaluate your trained model on the private validation and testing sets on the aforementioned eval.ai platform.

### ICText Dataset
For the latest version of the ICText dataset, images and annotations are sourced entirely from the training set of ViTrox-OCR and split into training and validation sets at a 7:3 ratio. You can refer to our upcoming paper for more information.

#### ICText Annotation JSON Format
```json
{
    "images": [
        {
        "id": int,
        "file_name" : str,
        "height": int,
        "width": int
        },
        ...
    ],
    "annotations": [
        {
        "id": int,
        "image_id": int,
        "bbox": [x1,y1,x2,y2,x3,y3,x4,y4],
        "category_id": int,
        "aesthetic": [one_hot_encoded],
        "area": float,
        "ignore": 0 or 1
        },
        ...
    ],
    "categories": [
        {
        "id": int, 
        "name": str
        }
        ...
    ]
}
```
Please note that the aesthetic label follows the sequence of low contrast, blurry, and broken. Given that the annotation for ICText follow MSCOCO's format closely, you can evaluate the predictions on ICText accordingly using this [COCO Results Analysis tool](https://github.com/chunchet-ng/COCO_results_analysis).

#### ICText Statistics
| Subset | Images | Perfect Text | Difficult Text | Total Text |
| :--- | :----: | :----: | :----: | :----: |
| Train | 7,000 | 19,153 | 50,597 | 69,750 |
| Test | 3,000 | 10,288 | 20,114 | 30,402 |

### ICText-LT Dataset
ICText-LT is a long-tailed character classification dataset introduced in the work of [FFDS-Loss](https://github.com/nwjun/FFDS-Loss/tree/main). Images are cropped from ICText and resampled to form this dataset.

#### ICText-LT Annotation TXT Format
```file_name, char_class```

#### ICText-LT Statistics
| Subset | Imbalance Factor | Images|
| :--- | :----: | :----: |
| Train | 18 | 68,307 |
| Train | 100 | 25,711 |
| Test | N/A | 6,300 |

## Attribute-Guided Curriculum Learning (AGCL)
In this work, we utilize the publicly available repositories for several object detection architectures:

1. [YOLOv4](https://github.com/Tianxiaomo/pytorch-YOLOv4)
2. [YOLOv5](https://github.com/ultralytics/yolov5)
3. [YOLOX](https://github.com/Megvii-BaseDetection/YOLOX)
4. [EfficientDet](https://github.com/zylo117/Yet-Another-EfficientDet-Pytorch)

The pre-trained weights are not released to protect the intellectual property of [ViTrox Corporation Berhad](https://www.vitrox.com/). However, a [Jupyter Notebook](agcl.ipynb) is provided to detail the implementation of AGCL, which can be easily integrated into the aforementioned object detectors. The code is written in PyTorch.

## Citation
If you wish to cite the ICDAR 2021 Competition on Integrated Circuit Text Spotting and Aesthetic Assessment paper and its participants' methods:

```bibtex
@inproceedings{icdar2021_ictext,
  author= {Ng, Chun Chet
  and Nazaruddin, Akmalul Khairi Bin
  and Lee, Yeong Khang
  and Wang, Xinyu
  and Liu, Yuliang
  and Chan, Chee Seng
  and Jin, Lianwen
  and Sun, Yipeng
  and Fan, Lixin},
  title= {ICDAR 2021 Competition on Integrated Circuit Text Spotting and Aesthetic Assessment},
  booktitle= {Document Analysis and Recognition -- ICDAR 2021},
  year= {2021},
  publisher= {Springer International Publishing},
  pages= {663--677}}
```

If you wish to cite the lastest version of the ICText dataset and AGCL:

***Our paper is currently under review. We will update this section when it is published.***

## Feedback
We welcome all suggestions and opinions (both positive and negative) on this work. Please contact us by sending an email to `ngchunchet95 at gmail.com` or `cs.chan at um.edu.my`.

## Acknowledgement
We would like to express our gratitude for the contributions of computing resources and annotation platforms by [ViTrox Corporation Berhad](https://www.vitrox.com/). Their generous support has made this work possible.

## License and Copyright
This project is open source under the BSD-3 license (refer to the [LICENSE](LICENSE.txt) file for more information).

&#169; 2023 Universiti Malaya.
