# 基于水色图像的水质评价
- 背景
	- 有经验的渔业生产的从业者可以通过观察水质变化调控水质，来维持养殖水体生态系统中的浮游植物、微生物、浮游动物等的动态平衡，然而这些判断是通过经验和肉眼观察得出的，存在主观性引起的观察性偏差，使观察结果的可比性、可重复性降低，不易推广使用。
	- 数字图像处理技术为计算机监控技术在水产养殖业的应用提供了更大的空间。在水质在线监测方面，数字图像处理技术是基于计算机视觉的，以专家经验为基础，对水色进行优劣分级，实现对水色的准确快速判别。
- 目标
	- 利用给出的已经分类的数据，利用图像处理技术实现水质自动评价。
- 分析
	- 通过拍摄得到的图像数据维度太大，不易分析，需要提取图像的特征，提取反映图像本质的关键指标，以达到自动进行图像识别或者分类的目的。
	- 显然，图像特征提取是图像识别和分类的关键步骤，提取的结果直接关系到图像识别和分类的好坏。
	- 图像特征
		- 主要包括颜色特征、纹理特征、形状特征和空间关系特征等。
		- 与几何特征相比，**颜色特征**更为稳健，对物体的大小和方向不敏感，表现出较强的鲁棒性。
		- 由于本案例中的水色图像是均匀的，所以主要关注颜色特征。
		- 颜色特征是一种全局特征，描述图像或者图像对应景物的表明性质。一般颜色特征是基于像素点的特征，所以图像区域的像素点都有自己的贡献。
		- 在利用图像颜色特征进行分析的研究中，实现方法上已经有啦很多研究成果，主要采用**直方图法**和**颜色矩方法**。
		- 其中直方图法反映颜色分布，即哪些颜色及其出现概率，优点是适合描述难以自动分割的图像和不需要考虑物体空间位置的图像；缺点是无法描述颜色的局部分布及每种色彩所处的空间位置，也就是无法描述图像中的某一具体的对象和物体。
		- 其中颜色矩法的数字基础为图像中的任何颜色分布均可以用它道德矩来表示。根据概率论，随机变量的概率分布可以由其各阶矩唯一描述和表示。一个图像的颜色分布完全可以看做是一种概率分布，图像就可以由各阶矩描述。颜色矩包含各个颜色的一阶矩、二阶矩和三阶矩，一个RGB图像有R、G、B三个通道，共9个分量。
		- 直方图法产生的特征维数一般会大于颜色矩产生的，为了避免过多变量干扰分类结果，使用颜色矩法。
- 处理过程
	- 数据获取
		- 给定分量图像集。
	- 数据探索
		- 由于是图像数据，进行观察探索即可。
	- 数据预处理
		- 图像切割
			- 不难发现，图像中的盛水容器干扰了特征分析，需要提取中心部分有意义的图像（定位101*101像素大小)。
			- 使用MATLAB等工具切割图像，得到切割后的图像。
		- 特征提取
			- 提取切割后的图像颜色矩，作为颜色特征，提取颜色矩时提取文件名中的类别和编号，得到数据集。
	- 数据挖掘建模
		- 按照传统机器学习中的82比划分训练集和测试集，使用**支持向量机**进行分类。
		- 同时应该注意到数据特征的区间极为接近[0-1]，，直接输入SVM中区分度很小，这里就应该走归一化的反路，将数据统一乘一个数值扩大区分度。
		- 这个数值过大过小都是不合适的。过小区分度过低，模型精确度差；过大则容易导致模型在训练样本中过拟合（这是很烦的一个问题）。可以根据测试准确率来选择合适的取值，经过反复测试选择30。
	- 后续处理
		- 建立模型后立业训练样本进行回判，得到混淆矩阵，判断是否可用。
		- 取测试样本输入，代入模型得到输出，得到混淆矩阵，判断模型可用价值。

- 补充说明
	- 由于拍摄图片资源过大，就不上传GitHub了，而且数字图像处理也并非我所擅长，我是直接使用分析图像所得数据集的。
	- 数据探索比较简单，也没有实现。
	- 参考书《Python数据分析与挖掘实战》