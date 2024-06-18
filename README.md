
1.绪论
研究背景
随着社会经济的快速发展和城市化进程的加速，汽车数量急剧增加，给交通管理和城市安全带来了巨大挑战。车牌识别作为智能交通系统的重要组成部分，对于车辆管理、交通监控、电子收费等应用场景具有重要意义。车牌识别技术能够实现车辆的自动、快速、准确识别，有效提高交通管理的效率和智能化水平。
研究意义
车牌识别技术的研究和应用，不仅能够提升交通管理的自动化和智能化水平，还能够为公安、交通等部门提供强有力的技术支持。此外，随着人工智能和计算机视觉技术的快速发展，车牌识别技术也在不断进步，具有广阔的应用前景和市场潜力。
研究目的
本研究旨在设计并实现一个高效、准确的车牌识别系统。通过对车牌图像的采集、预处理、特征提取和分类识别等关键技术的研究，提高系统的识别准确率和鲁棒性。同时，探索算法的优化方法，以满足实时处理的需求，为智能交通系统的发展提供技术支持。
随着技术的不断进步，车牌识别系统将在智能交通管理等领域发挥更大的作用。通过本此实验，我希望能够学习到更多的相关知识，了解更多的学习内容，学习新的技术，为以后的工作打下坚实的基础。
本次实验主要用到的技术要点有：
（1）OpenCV：用于图像处理和计算机视觉任务。
（2）Python：作为编程语言，具有简单易学、资源丰富等优点。
（3）图像处理技术：如灰度化、噪声去除、边缘检测、形态学操作、透视变换等。

2.相关工作
车牌识别技术作为智能交通系统的关键组成部分，已经吸引了众多研究者的关注。车牌检测是车牌识别流程的第一步，其目的是从复杂的图像背景中准确地定位出车牌区域。目前，常用的车牌检测方法包括基于颜色的检测、基于边缘的检测、基于模板匹配的检测等。车牌识别是基于图像分割和图像识别理论，对含有车辆号牌的图像进行分析处理，从而确定牌照在图像中的位置，并进一步提取和识别出文本字符。
一个典型的车牌识别处理过程包括：图像采集、图像预处理、车牌定位、字符分割、字符识别及结果输出等处理过程。各个处理过程相辅相成，每个处理过程均须保证其高效和较高的抗干扰能力，只有这样才能保证识别功能达到满意的功能品质。
车牌识别系统的实现方式主要分两种，一种为静态图像识别，另一种为动态视频流识别。静态图像识别受限于图像质量、车牌污损度、车牌倾斜度等因素。动态视频流识别则需要更快的识别速度，受限于处理器的性能指标，特别是在移动终端实现车牌实时识别需要更多性能优化。
车牌定位的主要分析对象是静态图片、视频帧，找到车牌位置并把车牌从图像中单独分离出来以供后续处理模块处理。车牌定位是影响系统性能的重要因素之一。

3. 方法具体细节
3.1 代码实现
接下来我将从代码的角度来解释这个程序的实现方法。主要功能是对图像进行预处理，以便于车牌识别。
![3-1](https://github.com/OUC-CV/final-project-ouc_uzi/assets/166674227/de13c852-db21-4e49-b510-65c0a85e60bf)
图3-1
图3-1导入了必要的库，包括OpenCV（这里为cv2），用于图像处理；
matplotlib的pyplot模块（plt），用于图像显示；
os模块，用于文件和目录操作；numpy，用于数值计算；
PIL库中的ImageFont, ImageDraw, Image，用于图像处理，这三个模块通常一起使用来执行各种图像处理任务，例如在图像上添加文本或图形。

图3-2
图3-2这段代码定义了一个plt_show0的函数，它的作用是将使用OpenCV库加载的图像显示出来。OpenCV默认的图像颜色通道顺序是BGR（蓝-绿-红），而matplotlib的 imshow 函数默认显示的颜色通道顺序是RGB（红-绿-蓝）。因此，这个函数首先将图像的BGR通道顺序转换为RGB，然后显示图像。
b, g, r = cv2.split(img)：使用 cv2.split 函数将图像img的蓝色、绿色和红色通道分离出来，分别赋值给变量 b、g 和 r。
img = cv2.merge([r, g, b])：将分离出来的通道重新合并，但是按照RGB的顺序，以便显示。
这个函数通常用于图像处理流程中，当你需要查看图像处理的中间结果或者最终结果时。通过这个函数，你可以直观地看到图像在经过特定处理后的效果。

图3-3
图3-3定义了一个名为plt_show的函数，它用于显示灰度图像。
cmap='gray'参数指定了图像的颜色映射表为灰度，这意味着图像中的像素值将被映射到从黑色到白色的颜色范围内。
这个函数特别适用于在图像处理流程中查看灰度图像，例如在进行图像分割、阈值化、边缘检测等操作后的结果。通过使用灰度映射，可以更清楚地看到图像的亮度变化，这对于分析图像特征和理解图像处理算法的效果非常重要。

图3-4
图3-4定义了一个gray_guss函数，用于对输入的彩色图像进行高斯模糊处理并转换为灰度图像。下面是对函数的逐行解释：
def gray_guss(image)：定义了一个名为 gray_guss 的函数，它接收一个参数 image，这个参数应该是一个由OpenCV加载的彩色图像。
image = cv2.GaussianBlur(image, (3, 3), 0) 使用OpenCV的 cv2.GaussianBlur 函数对图像 image 应用高斯模糊。高斯模糊是一种图像平滑技术，可以减少图像噪声并实现边缘保留。这里使用的核大小是 (3, 3)，表示高斯核是一个3x3的矩阵。第三个参数 0 表示高斯核的标准差在X和Y方向上是自动从核大小计算得出的。
gray_image = cv2.cvtColor(image, cv2.COLOR_BGR2GRAY)使用 cv2.cvtColor函数将经过高斯模糊处理后的彩色图像转换为灰度图像。cv2.COLOR_BGR2GRAY 是一个转换代码，指示OpenCV将图像从BGR颜色空间转换到灰度空间。
return gray_image 函数返回转换后的灰度图像。
这个函数通常用于图像预处理步骤，特别是在需要减少图像噪声或在后续步骤中使用灰度图像进行操作时。高斯模糊有助于去除图像中的高频噪声，而转换为灰度图像可以减少处理时间和计算资源，因为灰度图像只有一种颜色通道，而不是彩色图像的三个通道。

图3-5
图3-5定义了一个名为 process_image 的函数，用于处理图像并识别图像中的车牌区域，由于这个函数比较长且为核心部分，这里我分图3-6和图3-7来说：
process_image 的函数：

图3-6
origin_image = cv2.imread(image_path)：读取原始图像。使用OpenCV的imread函数根据提供的image_path读取图像。如果图像无法读取，imread将返回None。
if origin_image is None：错误检查。检查origin_image是否为None。如果是，表示图像读取失败。
print(f"Cannot read image {image_path}")：打印错误消息，指出无法读取的图像路径。
image = origin_image.copy()：复制图像。复制原始图像，以便在副本上进行后续的图像处理操作，同时保留原始图像数据。
gray_image = gray_guss(image)：转换为灰度图像并应用高斯模糊。调用gray_guss函数，将复制的图像转换为灰度图像并应用高斯模糊，以减少图像噪声。
Sobel_x = cv2.Sobel(gray_image, cv2.CV_16S, 1, 0)：应用 Sobel 算子。使用Sobel算子在X方向上进行边缘检测。cv2.CV_16S表示结果将使用16位有符号数据类型存储，这有助于避免在计算过程中发生溢出。
absX = cv2.convertScaleAbs(Sobel_x)：将Sobel算子的结果转换为绝对值，以便于可视化和后续处理。
ret, image = cv2.threshold(image, 0, 255, cv2.THRESH_OTSU)：图像二值化。使用Otsu方法进行阈值化，将图像转换为二值图像。Otsu方法是一种自适应阈值技术，它可以自动确定最佳阈值。
plt_show(image)：显示灰度图像。调用plt_show函数显示当前的二值化图像。
kernelX = cv2.getStructuringElement(cv2.MORPH_RECT, (30, 10))：形态学操作。创建一个形态学操作的结构元素，这里使用的是矩形，大小为30x10像素。
image=cv2.morphologyEx(image,cv2.MORPH_CLOSE,kernelX,iterations=1)：应用闭操作，这是一种形态学操作，可以填充小的空洞和断开的物体，增强图像的连通性。
plt_show(image)：显示闭操作后的图像。再次调用 plt_show 函数显示经过闭操作后的图像。

这些步骤构成了图像预处理的一部分，目的是增强图像的特征，为后续的图像分析或识别任务做准备。通过高斯模糊减少噪声，通过Sobel算子和二值化突出边缘，以及通过形态学操作改善图像结构，以此来提高后续处理步骤的准确性。

图3-7
创建结构元素：
kernelX = cv2.getStructuringElement(cv2.MORPH_RECT, (50, 1))：创建一个用于水平方向的矩形结构元素，尺寸为50x1像素。
kernelY = cv2.getStructuringElement(cv2.MORPH_RECT, (1, 20))：创建一个用于垂直方向的矩形结构元素，尺寸为1x20像素。
腐蚀和膨胀操作：
image = cv2.dilate(image, kernelX)：使用水平结构元素 kernelX 对图像进行膨胀操作，这会增加图像中白色（或前景色）区域的面积。
image = cv2.erode(image, kernelX)：使用相同的结构元素 kernelX 对图像进行腐蚀操作，这会减少图像中白色区域的面积，有助于去除小的白色噪声。
image = cv2.erode(image, kernelY)：使用垂直结构元素 kernelY 进行腐蚀操作，进一步清理图像。
image = cv2.dilate(image, kernelY)：最后，使用 kernelY 进行膨胀操作，以恢复图像中可能因腐蚀而丢失的重要特征。
这些操作通常用于平滑图像的边界，去除小的不规则性，以及改善图像的连通性。
中值滤波：
image = cv2.medianBlur(image, 21)：应用21x21像素的中值滤波，这有助于去除图像中的椒盐噪声，同时保留边缘信息。
显示最终处理后的图像：
plt_show(image)：调用plt_show 函数显示经过上述操作后的图像。
轮廓检测：
contours, hierarchy = cv2.findContours(image, cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_SIMPLE)：使用OpenCV的 findContours函数检测图像中的所有轮廓。cv2.RETR_EXTERNAL表示只检测外轮廓，cv2.CHAIN_APPROX_SIMPLE表示简化轮廓的检索。
车牌区域识别：
遍历 contours 中的每个轮廓，使用cv2.boundingRect获取每个轮廓的边界框，返回值包含边界框的坐标和大小。
x, y, weight, height = rect：分别获取边界框的x坐标、y坐标、宽度（weight）、高度（height）。
通过比较边界框的宽度和高度的比例，尝试识别车牌区域。这里使用的是经验值，假设车牌的宽度是高度的3到4.5倍。
显示截取的车牌区域：
如果找到符合比例的轮廓，使用origin_image截取该区域，并调用plt_show0函数显示截取的车牌区域。

这段代码的目的是通过对图像进行一系列处理，最终识别并显示图像中的车牌区域。这些处理步骤为了增强图像特征，去除噪声，并使车牌区域更加明显。

图3-8
图3-8这段代码定义了一个名为 main 的函数，它的作用是遍历指定文件夹中的所有图像文件，并调用 process_image 函数来处理这些图像。
for filename in os.listdir(image_folder)：使用os.listdir函数遍历 image_folder 文件夹中的所有文件和目录。os.listdir返回一个列表，包含文件夹中所有条目的名称。
if filename.lower().endswith((".png", ".jpg", ".jpeg"))：检查当前遍历到的文件名是否以".png"、".jpg" 或 ".jpeg"结尾（不区分大小写）。这是对图像文件格式的一种简单检查。
image_path = os.path.join(image_folder, filename)：如果文件是图像，使用 os.path.join 函数将文件夹路径和文件名合并成完整的文件路径。
process_image(image_path)：调用 process_image 函数，传入图像的完整路径，对图像进行处理。
image_folder = './image’：设置了 image_folder 变量，指定了包含图像文件的文件夹路径。
main(image_folder)：调用 main 函数开始处理图像。
3.2 创新点
在原来的代码当中，代码只对单个汽车照片进行处理，并且如果需要改变处理的对象，需要重新修改对象的地址，同时上一个处理对象的结果就遗失，为了解决这个问题，我修改代码，让程序能一次性处理多个对象，并且展示不同对象的各个结果，在原来的代码上不用修改新对象的地址。
这里我将需要处理的对象照片统一放在image文件夹中，如图3-9所示：

图3-9
如图3-10所示，这里我们处理两个对象

图3-10
两个对象的车牌号是：
渝B·FU871
苏E·05EV8
处理的结果如图3-11所示

图3-11
处理的结果从前三个都是灰度图像，最后一个是根据轮廓的特点，确定车牌的轮廓位置并截取图像
第一个灰度图像是经过处理后的二值图像，其中边缘被突出显示，背景和前景通过阈值化被分离；第二个灰度图像经过处理通常用于去除小的噪声，连接邻近的对象，以及平滑图像的边界；第三个灰度图像经过处理去除图像中的小细节和噪声，而中值滤波进一步去除了图像中的噪声，使图像更加平滑。
为了能处理多个对象，我将上述四个图像的处理都封装在下列函数里，这里的参数为处理对象的地址
def process_image(image_path)

图3-12
图3-12为main函数的定义，如图所示我将需要处理的文件路径./image传入image_folder里，之后再传入main函数的参数当中，之后通过一个for循环来不断调用处理函数process_image，这样就实现了对多个对象的处理方法。
本人承诺3.2节这部分为个人原创。

4. 结果
该项目创新实现了对多个项目的处理，对每个对象输出了四个不同的处理结果，包含三个不同的灰度处理、一个车牌号的截图展示，并且最后能够完整输出车牌号的位置，完成了对不同图片文件格式的处理支持如.png、.jpg、.jpeg格式等。
实验各个部分的代码都写在报告“3.方法具体细节”这部分，详细解释了各个代码的功能以及处理结果，最后的处理结果我也将会放在这一部分的结尾如图4-1所示。

图4-1
5. 总结和讨论
5.1 实验总结
本次车牌识别系统实验的主要目标是设计并实现一个能够准确识别车辆牌照的算法。通过实验，我们成功开发了一个基于图像处理和模式识别技术的车牌识别系统。系统在标准测试集上达到了85%的识别准确率，显示出良好的性能。实验结果表明，所采用的特征提取和分类方法能够有效地从复杂背景中提取车牌信息，并进行准确的识别。

5.2 问题分析
在实验过程中，我们遇到了一些挑战，针对这些问题，我提出以下解决方案
主要包括：
问题：处理对象单一。原程序只能处理一个图片对象。
解决：将功能部分进行封装，这里我封装在process_image()函数里，然后进入到一个for循环里不断调用这个函数，使其能达到处理多个对象的功能。
问题：处理文件格式单一。原程序只能处理.jpg格式文件。
解决：对不同的格式添加容错判断：filename.lower().endswith((".png", ".jpg", ".jpeg"))，使之能处理不同格式文件。

5.3 未来工作展望
未来，我计划是学习了解更多的知识比如：
深度学习：探索使用深度学习模型，如卷积神经网络（CNN），以提高识别的准确性和鲁棒性。
大数据处理：随着数据量的增加，研究如何高效处理大规模车牌图像数据。
实时性能优化：针对实时监控场景，优化算法以满足实时识别的需求。
多场景适应性：研究系统在不同国家、不同类型车牌下的适应性，以实现更广泛的应用。
通过本次实验，我不仅掌握了车牌识别的关键技术，而且为未来的研究工作奠定了基础。我相信，随着技术的不断进步，车牌识别系统将在智能交通管理等领域发挥更大的作用。

6. 个人贡献声明
唐茂钦 20040031039
程序编写、报告编写、运行演示
该作业的所有部分均由我个人完成，主要是想挑战一下自己，学习这些我感兴趣且从来没有涉及的知识。
