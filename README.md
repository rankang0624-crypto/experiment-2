# 计算机视觉实验二：图像增强实验报告

# 一、实验目的

学习 OpenCV 的基本使用方法，利用 OpenCV、scikit-image、NumPy 和 Matplotlib 对图像进行颜色空间转换、噪声添加、平滑与滤波，实现图像增强；比较均值滤波、中值滤波和高斯滤波对不同噪声的处理效果，并手动实现彩色图像中值滤波。

# 二、实验环境

Python；OpenCV；NumPy；Matplotlib；scikit-image。原始图像采用本次实验上传的玩偶图片。

# 三、实验内容与代码输出结果

3.1 导入依赖库

首先导入实验所需的 OpenCV、scikit-image、NumPy 和 Matplotlib 库。

    import cv2
    from skimage.util import random_noise
    import numpy as np
    from matplotlib import pyplot as plt
    import os

3.2 读取原始图像并获取像素值

使用 OpenCV 读取本次上传的原始图片。图像尺寸为 1169×1181，通道数为 3。读取 [100,100] 像素点后，程序实际输出 BGR = (251, 243, 183)，RGB = (183, 243, 251)。

    img = cv2.imread(
    "7dcf7830-7509-46bd-b302-b6ac5adf0f33.jpg"
    )

    b, g, r = img[100, 100]

    print("图像尺寸：", img.shape)
    print("BGR：", (b, g, r))
    print("RGB：", (r, g, b))

    代码实际输出：

    图像尺寸：(1181, 1169, 3)
    BGR：(251, 243, 183)
    RGB：(183, 243, 251)

<img width="1322" height="1378" alt="image" src="https://github.com/user-attachments/assets/663c64c4-19ff-45c4-8736-75bd9275d21f" />

3.3 BGR → RGB 颜色空间转换

OpenCV 读取图像默认采用 BGR 通道顺序，使用 cv2.cvtColor() 转换为 RGB 后再利用 Matplotlib 显示。

    rgb_img = cv2.cvtColor(
    img,
    cv2.COLOR_BGR2RGB
    )

    plt.figure(figsize=(8, 8))
    plt.imshow(rgb_img)
    plt.title("RGB Image")
    plt.axis("off")
    plt.show()
<img width="1322" height="1378" alt="image" src="https://github.com/user-attachments/assets/6aa1237f-9431-4329-aff1-420bd43f25b3" />

3.4 灰度图像转换

使用 cv2.COLOR_BGR2GRAY 将三通道彩色图像转换为单通道灰度图。

gray_img = cv2.cvtColor(
    img,
    cv2.COLOR_BGR2GRAY
)

plt.figure(figsize=(8, 8))
plt.imshow(gray_img, cmap="gray")
plt.title("Gray Image")
plt.axis("off")
plt.show()
<img width="1322" height="1378" alt="image" src="https://github.com/user-attachments/assets/28d276b7-95be-48c3-9476-053dfc9c85e2" />

3.5 添加椒盐噪声和高斯噪声

使用 random_noise() 分别向原始 RGB 图像添加椒盐噪声和高斯噪声。本次程序固定随机种子为 42，以保证结果可以复现。

np.random.seed(42)

# 椒盐噪声
    sp_noise_img = random_noise(
    rgb_img,
    mode="s&p",
    amount=0.4
    )

# 高斯噪声
    gus_noise_img = random_noise(
    rgb_img,
    mode="gaussian",
    mean=0.2,
    var=0.03
    )

    plt.figure(figsize=(15, 5))
    plt.subplot(1, 3, 1)
    plt.imshow(rgb_img)
    plt.title("Original Image")
    plt.axis("off")

    plt.subplot(1, 3, 2)
    plt.imshow(sp_noise_img)
    plt.title("S&P Noise")
    plt.axis("off")

    plt.subplot(1, 3, 3)
    plt.imshow(gus_noise_img)
    plt.title("Gaussian Noise")
    plt.axis("off")

    plt.tight_layout()
    plt.show()
<img width="2945" height="1017" alt="image" src="https://github.com/user-attachments/assets/470237d9-965f-432b-8572-21feae868edf" />

3.6 均值、中值和高斯滤波

采用 5×5 窗口分别进行均值滤波、中值滤波和高斯滤波，并分别处理椒盐噪声和高斯噪声。

# 转换为 uint8
    sp_noise_uint8 = (sp_noise_img * 255).astype(np.uint8)
    gus_noise_uint8 = (gus_noise_img * 255).astype(np.uint8)

# 均值滤波
    mean_sp = cv2.blur(sp_noise_img, (5, 5))
    mean_gus = cv2.blur(gus_noise_img, (5, 5))

# 中值滤波
    mid_sp = cv2.medianBlur(sp_noise_uint8, 5)
    mid_gus = cv2.medianBlur(gus_noise_uint8, 5)

# 高斯滤波
    gauss_sp = cv2.GaussianBlur(
    sp_noise_uint8, (5, 5), 0
    )
    gauss_gus = cv2.GaussianBlur(
    gus_noise_uint8, (5, 5), 0
    )
    plt.figure(figsize=(14, 9))

    plt.subplot(2, 3, 1)
    plt.imshow(mean_sp)
    plt.title("S&P + Mean")
    plt.axis("off")

    plt.subplot(2, 3, 2)
    plt.imshow(mid_sp)
    plt.title("S&P + Median")
    plt.axis("off")

    plt.subplot(2, 3, 3)
    plt.imshow(gauss_sp)
    plt.title("S&P + Gaussian")
    plt.axis("off")

    plt.subplot(2, 3, 4)
    plt.imshow(mean_gus)
    plt.title("Gaussian + Mean")
    plt.axis("off")

    plt.subplot(2, 3, 5)
    plt.imshow(mid_gus)
    plt.title("Gaussian + Median")
    plt.axis("off")

    plt.subplot(2, 3, 6)
    plt.imshow(gauss_gus)
    plt.title("Gaussian + Gaussian")
    plt.axis("off")

plt.tight_layout()
plt.show()
<img width="2679" height="1778" alt="image" src="https://github.com/user-attachments/assets/11659e0f-cb38-4883-ac38-2a483691c1ce" />

3.7 滤波结果的代码输出

为辅助比较滤波结果，计算各结果相对于无噪原图的 MSE。MSE 数值越小，表示像素差异越小。

    def mse(a, b):
    a = np.asarray(a).astype(np.float32)
    b = np.asarray(b).astype(np.float32)
    return np.mean((a - b) ** 2)

    print("MSE结果：")
    print("椒盐+均值:", mse(mean_sp * 255, rgb_img))
    print("椒盐+中值:", mse(mid_sp, rgb_img))
    print("椒盐+高斯:", mse(gauss_sp, rgb_img))
    print("高斯+均值:", mse(mean_gus * 255, rgb_img))
    print("高斯+中值:", mse(mid_gus, rgb_img))
    print("高斯+高斯:", mse(gauss_gus, rgb_img))

    MSE结果：
    椒盐+均值: 1583.19
    椒盐+中值: 46.03
    椒盐+高斯: 1858.96
    高斯+均值: 2390.75
    高斯+中值: 2482.65
    高斯+高斯: 2393.65

3.8 手动实现彩色中值滤波

按照参考文档的实验方法，分别对 R、G、B 三个通道进行处理，在每个像素位置建立 5×5 邻域窗口，计算窗口内像素的中值。

    def manual_median_filter_color(image, kernel_size=5):
    pad = kernel_size // 2
    filtered_img = np.zeros_like(image)

    for c in range(3):
        channel = image[:, :, c]
        padded_channel = np.pad(
            channel,
            pad_width=pad,
            mode="edge"
        )

        for i in range(channel.shape[0]):
            for j in range(channel.shape[1]):
                region = padded_channel[
                    i:i + kernel_size,
                    j:j + kernel_size
                ]
                filtered_img[i, j, c] = np.median(region)

    return filtered_img

    manual_mid = manual_median_filter_color(
    sp_noise_uint8,
    kernel_size=5
    )


# 实际运行输出
手动中值滤波运行时间：5.35 秒

<img width="2344" height="1213" alt="image" src="https://github.com/user-attachments/assets/87f31549-26ae-4126-ba2d-e159f892b90c" />


# 四、实验结果与分析

1. 原始图像与颜色空间转换：程序成功读取原始彩色图像。OpenCV 读取结果为 BGR 顺序，转换为 RGB 后可以正确显示颜色。灰度化后图像保留了主要亮度信息。

2. 噪声添加效果：椒盐噪声在图像中形成明显的随机突变点，对局部像素破坏较强；高斯噪声表现为较连续的随机偏差，对整幅图像产生较均匀的影响。

3. 滤波效果：椒盐噪声下，中值滤波的 MSE 为 46.03，明显低于均值滤波的 1583.19 和高斯滤波的 1858.96，说明中值滤波对本实验椒盐噪声的恢复效果最好。

4. 高斯噪声下，本次采用的参数和 MSE 指标显示三种滤波结果均存在一定误差，因此不能仅凭 MSE 判断所有视觉效果；应结合滤波后的图像观察平滑程度、细节和边缘保持情况。

5. 手动中值滤波通过逐通道、逐像素和逐窗口计算中值，加深了对中值滤波工作原理的理解。

# 五、实验小结

本实验完成了彩色图像的读取、BGR→RGB 转换、灰度化、椒盐噪声与高斯噪声添加，并采用均值滤波、中值滤波和高斯滤波进行去噪比较。实验结果表明，中值滤波对椒盐噪声具有明显的处理优势，能够有效降低孤立噪点并较好保持图像边缘；均值滤波和高斯滤波具有较好的平滑作用。通过手动实现彩色中值滤波，进一步理解了滤波窗口、边缘填充、邻域像素和中值计算的基本过程。
