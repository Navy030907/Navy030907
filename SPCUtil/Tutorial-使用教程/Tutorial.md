**This document is written in Chinese. If you don’t know Chinese, please use a translation software to translate me.**

# SPC-Util（SU）

SPC-Util，简称 SU ，是一款专门为**PvZ2**定制的【静态】修改器（直接改动游戏运行所依赖的文件）。

功能丰富，涵盖所有PvZ2修改工具所拥有的、没有的功能，例如 PvZ2Tool，以及SU作者以前制作的 PvZTool 中的各个功能。

例如 RTON文件的编码解码、**PAM文件与FLASH动画的相互转换** **RSGP与RSB数据包(安卓设备中保存为OBB文件)的解包(拆包)与打包(回包)**、PTX图像文件与PNG图像的转换、PNG图像的分割、等等。

程序只接受读取 UTF-8 编码的文本，使用其他编码格式的文本文件将会导致程序错误，务必确保操作的文本是 纯ASCII 或 UTF-8 编码格式。

**注意：程序只能始终在同一驱动器下运行，默认情况下，程序在本体（exe文件）所在驱动器中运行。**

## 一些说明

1.4版本 新增了 **RSB-Patch** 功能，意为 RSB补丁，能快速地对一份修改进行多平台、多版本地移植。是**修改RSB的最佳方案**。

> * 1.11.0 版本开始，已解除所有打包限制、开放BatchMode，务必使用最新版本

# 依赖文件

SU使用的特有文件：

* **suc** : SPC-Util Config 配置文件。是程序必须依赖、加载的，不建议改动。

* * Config.suc : 标准Config文件，用户可再次基础是自定义QE选项。

* **sus** : SPC-Util Script 脚本文件。SU的操作都需要通过脚本来完成，脚本是SU处理用户需求的基础单位。

* **surpc** : SPC-Util RSB-Patch-Config RSB补丁配置文件。是运行RSB补丁功能的依据。

# 注册表（Windows）

Windows平台下的SU会为程序运行添加必要的注册表信息，无完整参数情况下运行（直接运行、拖拽文件等）时，SU从注册表中读取所记录的默认参数。

# 环境配置

将 Config.suc 文件放置于D盘下文件夹：\SPCUtil中，即能够让程序在所在驱动器下找到 \SPCUtil\Config.suc 文件。

# 基本使用

* 直接打开程序(exe)，程序会根据注册表提供的默认参数运行。

	> 默认注册表下，程序只执行Config中的预准备脚本(Intro/Outro-Script)。

* **QuickEnter** 简称QE，用文件路径作为参数。

	QE是SU 1.3.0版本中增加的功能，能使用户一定程度上脱离"写脚本"的方式控制程序运行，快捷易用。程序通过 Config.suc 文件内预设的脚本筛选可用功能。

	QE有两种使用情景：

	1. 用SU打开文件/文件夹：选中目标文件/文件夹，右击菜单中选择 打开方式 选择使用 SU 打开。

	2. 拖拽文件/文件夹（数目不限）到exe图标打开，即可让程序以所选内容为参数运行。

	QE具体使用流程如下：

	1. 按上述两种方式启动SU。

	2. 程序按照QE配置筛选可用功能（主要是通过后缀名进行筛选），将可用功能及其序号打印在控制台界面上，并提示： ` InPut Number To Select Option : ` 用户输入需要使用的功能的序号，回车后即可运行对应功能。

	说明：

	* 当程序找不到这个文件可用的功能时，会出现 ERROR ，提示 “未找到可用功能” 并终止程序。

	* 当参数为一个脚本文件（后缀为sus）时，QE会直接读取文件内容为脚本，直接运行该脚本。

	* **BatchMode(批处理模式)**: 当参数为文件夹时，会出现以下提示： ` Is Batch Mode ? ` 询问是否要使用BatchMode对文件夹内文件进行批处理，通过键入数字0（不使用）或非0（使用）选择。

		批处理模式下，程序通过判定条件（后缀名、文件名、等），为QE配置中提供的待选功能筛选可处理文件，通过键入功能序号选择需要使用的功能，并将处理结果保存在同级的 "**~???**" 文件夹中， "???" 为输入的文件夹名称。

		可在QE配置文件中设置始终不使用批处理模式。

# 程序运行时的脚本变量输入

在脚本中，可以定义需要 运行时用户输入 的一些 脚本中变量 ，这能够增加脚本的灵活性、适用范围，控制脚本的运行过程。

程序运行过程中，当运行到脚本定义的 运行时用户输入变量 时，将会暂停程序，并显示以 ? 问号开头的提示，例如 ` ? Bool : ` ，用户需要根据提示输入对应的 有效数值 使程序将 用户输入值 赋予该变量，使程序继续执行脚本。

以下是主要的提示及需要进行的操作：

* ` ? Bool ` : 变量是一个逻辑值（真 或 假），输入数字 0 表示 假， 输入数字 1 表示 真。输入完毕后按回车键。

	> 0 // 逻辑值 假 作为变量值。

* ` ? Int ` : 变量是一个无符号整数，输入一个无符号的数字（真即 无 正负号，只由 0~9 的数字组成）。输入完毕后按回车键。

	> 123 // 无符号整数 100 假 作为变量值。

* ` ? Int_Hex ` : 变量是一个无符号整数，输入十六进制的数字（由 0~9 | A~Z 组成，无 0x前缀）。输入完毕后按回车键。

	> 7F // 无符号整数 0x7F（十进制为 127） 作为变量值。

* ` ? ByteSize_Str ` : 变量是一个无符号整数，输入表示字节数大小的字符串（由两部分组成：无符号数字 与 表示单位的字符 B、K、M、G）。输入完毕后按回车键。

	> 10M // 无符号整数 ‭10485760（单位为兆字节(1024 * 1024)，10M 表示 十兆字节）‬ 假 作为变量值。

* ` ? Str ` : 变量是一个字符串，输入一行不包含空格的字符串。输入完毕后按回车键。

	> abcde // 字符串 "abcde" 作为变量值。

	> abc de // 字符串 "abc" 作为变量值。"de"因为被空格分割而不作为此次输入，会在下次输入时被读取，可能导致程序崩溃，切勿输入带空格的字符串。

* ` ? Path_Str ` : 变量是一个路径，输入一行不包含空格的字符串（表示相对于当前目录或是根目录的路径）。输入完毕后按回车键。

	> DIR_A\FILE_B // 路径 "DIR_A\FILE_B"( [ "DIR_A", "FILE_A" ] ) 作为变量值。表示当前目录(Current-Work-Directory)下文件夹 DIR_A 中的文件 FILE_B 的路径。

	> \DIR_A\FILE_B // 路径 "\DIR_A\FILE_B"( [ "", "DIR_A", "FILE_A" ] ) 作为变量值。表示根目录(Root)下文件夹 DIR_A 中的文件 FILE_B 的路径。

* ` ? Path_Dir ` : 变量是一个路径，程序将弹出 文件选择 窗口，选择一个文件，程序将获取其 父文件夹路径 作为输入值。

* ` ? Path_File ` : 变量是一个路径，程序将弹出 文件选择 窗口，选择一个文件，程序将获取 该文件路径 作为输入值。

* ` ? PathList_Dir ` : 变量是一个路径数组，程序将弹出 文件选择 窗口，选择一个文件，程序将获取其 父文件夹路径 ，并获取该 文件夹内所有子文件夹路径（相对于父文件夹） 作为输入值。

* ` ? PathList_File ` : 变量是一个路径数组，程序将弹出 文件选择 窗口，选择一个文件，程序将获取其 父文件夹路径 ，并获取该 文件夹内所有子文件路径（相对于父文件夹） 作为输入值。

* ` ? PathList_Dir_Abs ` : 变量是一个路径数组，程序将弹出 文件选择 窗口，选择一个文件，程序将获取其 父文件夹路径 ，并获取该 文件夹内所有子文件夹路径（相对于根目录） 作为输入值。

* ` ? PathList_File_Abs ` : 变量是一个路径数组，程序将弹出 文件选择 窗口，选择一个文件，程序将获取其 父文件夹路径 ，并获取该 文件夹内所有子文件路径（相对于根目录） 作为输入值。

# 简单应用

* RTON 与 JSON 的相互转换 : 同上，通过QE输入后缀为 rton 或 json 的文件，可以看到以下内容：

	```
	~  ?? : RTON-EnCode // 将 RTON 解码为 JSON
	~       In  : file - JSON
	~       Out : file - RTON
	```
	```
	~  ?? : RTON-DeCode // 将 JSON 编码为 RTON
	~       In  : file - RTON
	~       Out : file - JSON
	```

	其中，"??"为该功能的序号，输入序号后，程序将以该 通过输入的 rton | json 文件 转换处 json | rton 文件，并输出到同一文件夹内。

* RSGP、RSB数据包 的 解包、打包（回包） : 运行 SPCUtil_Config.zip 内包含的 "RSB打包辅助脚本" 文件夹内提供的对应的脚本文件，按程序提示操作即可。

# 高级应用

## 在 RSGP 中添加或删除 资源文件

解包 RSGP 后，可在输出目录内看到 StructInfo.json 文件。打包时将通过其内容，将 Res 文件夹内的 资源文件 打包为一个 RSGP。

添加或删除资源都要修改StructInfo.json

在 Res 中添加或删除 Res-Object 以增删资源群。

需在 Res 文件夹 中添加新增的文件，完成后使用打包脚本打包为 RSGP 即可。

参考内容如下：

```
{
	"HeaderType": 4,
	// 始终为 4 ，不要更改。

	"CompressMethod": [ true, true ],
	// 是否使用 ZLIB 压缩资源数据段。第一个逻辑值表示是否压缩普通资源数据，第二个逻辑值表示是否压缩图像资源数据。

	// Res 数组，其成元素是 Res-Object，表示了包含在RSGP内的所有资源。
	"Res": [ {

			"Path": [ "PACKAGES", "FONTS.RTON" ]
			// 资源文件的路径，相对于 RSGP文件夹 中的 Res 文件夹。表示 Res 文件夹下 的 PACKAGES/FONTS.RTON 文件。

			// 只有 Path 这一成员，将会判定为 普通资源 ，数据保存在 普通资源数据段 。

		}, {

			"Path": [ "ATLASES", "ALWAYSLOADED_1536_00.PTX" ],
			// 资源文件的路径，相对于 RSGP文件夹 中的 Res 文件夹。表示 Res 文件夹下 的 ATLASES/ALWAYSLOADED_1536_00.PTX 文件。

			"AtlasInfo": {
				// 图像信息，拥有这一成员的 Res-Object 将会判定为 图像资源 ，数据保存在 图像资源数据段 。

				"Idx": 0,
				// 索引值，用于为图像资源绑定 RSB-AtlasInfo-List 中记录的 宽高、纹理格式。

				"Size": [ 1024, 1024 ]
				// 图像资源的 宽 与 高 ，需与对应的 RSB-AtlasInfo保持一致。

			}
		} ]
}
```

## 在 RSB(数据包) 中添加或删除 资源群、资源子群、资源文件

解包 RSB 后，可在输出目录内看到 StructInfo.json 文件。打包时将通过其内容，将 Res | Packet 文件夹内的文件打包为一个 RSB。

添加或删除资源都要修改StructInfo.json

在 Group数组 中添加或删除 Group-Object 以增删资源群。
在 某个Group 中添加或删除 SubGroup-Object 以增删资源子群。
在 某个SubGroup 中添加或删除 Res-Object 以增删资源文件。

需在 Res 文件夹 中添加新增的文件，完成后使用打包脚本打包为 RSGP 即可。

参考内容如下：

```
{

	"HeaderType": 4,
	// 始终为 4 ，不要更改。

	"SetResListInHeader": false,
	// 始终为 false ，不要更改。

	"UseMoreAtlasInfo": false,
	// 使用拓展的图像信息(AtlasInfo-Expand)，安卓中文版数据包需要打开该选项。

	// Group Map，其成员的名(Key)表示群ID(Group-ID)，其值(Value)是 Group-Object。表示了包含在RSB内的所有资源群。
	"Group": {

		// 一个ID为 AlwaysLoaded 的资源群。
		"AlwaysLoaded": {

			// 这是一个复合群，复合群中可以包含多个子群，游戏通过不同的 Category 属性有选择地加载。
			"IsComposite": true,

			// SubGroup Map，其成员的名(Key)表示子群ID(SubGroup-ID)，其值(Value)是 SubGroup-Object。表示了包含在该群内的所有资源子群。
			"SubGroup": {

				// 一个ID为 AlwaysLoaded_1536 的资源子群。
				"AlwaysLoaded_1536": {

					// 分类属性。游戏通过子群的分类属性与当前环境对比，相符时才会加载这个子群。当两个值均为 空值(null) 时，游戏始终加载这个子群。
					// 第一个值为 ResID (无符号整数)，不同性能的设备加载不同的 Res，好的设备能够加载 ResID = 1536 的子群，使用高清图像，差些的设备只能加载 ResID = 768 (或更小) 的资源，使用较低清图像。可为空值(null)。
					// 第二个值为 LocID (四个字符的字符串)，游戏通过当前语言环境加载不同的子群，例如，美式英语环境下，游戏加载 LocID = ENUS 的子群。可为空值(null)。
					"Category": [ 1536, null ],
					// 这是一个 ResID = 1536 的子群，只有适用 高清图像 的设备会加载它。

					// 以下内容与 RSGP 一节中提供的参考一致
					"HeaderType": 4,
					"CompressMethod": [ true, true ],
					"Res": [ {
							"Path": [ "ATLASES", "ALWAYSLOADED_1536_00.PTX" ],
							"AtlasInfo": {
								"Idx": 0,
								"Size": [ 1024, 1024 ],

								"TexFmt": 0
								// RSB的StructInfo中特有的属性，无符号整数，表示这个图像资源(PTX)使用的纹理格式。常用取值如下：
								// 0 : RGBA8888 (安卓平台) 或 BGRA8888 (iOS平台)
								// 30 : PVRTC-4BPP (iOS平台)
								// 147 : ETC1+ALPHA (安卓平台)
							}
						} ]
				},
				"AlwaysLoaded_768_ENUS": {
					"Category": [ 768, "ENUS" ],
					// 这是一个 ResID = 384 并且 LocID = "ENUS" 的子群，只有适用 较低清图像 的设备在设备语言为 美式英语 时才会加载它。

					// 省略内容
				},
				"AlwaysLoaded_Common": {
					"Category": [ null, null ],
					// 这是一个通用子群，不管什么环境下，设备都会加载它。

					// 省略内容
				}
			}
		}
	}
}
```

## RSB-PATCH 为 RSB数据包 制作补丁

1. 新建 后缀为 patch 的文件夹，例如 mod.patch 。这称为 Patch文件夹 。

2. 在这个文件夹内创建以下内容：

	* Config.surpc 文件 : 配置信息文件。

	* ResInfo 文件夹 : 存放外部 ResInfo 。

	* StructInfo 文件夹 : 存放外部 StructInfo 。

	* Res 文件夹 : 存放需要增加或修改到数据包内的文件。例如 在Res文件夹内存放 packages/fonts.rton 文件，程序会根据配置信息，寻找到这个文件并复制到目标数据包目录中。

3. 修改配置信息文件。参考内容如下：

```
{
	// 存在则跳过 PassIfExist、始终重写 OverWrite、保留原数据基础 BaseOn(default)

	// 对 数据包的 ResInfo 与 StructInfo 进行修改
	"Group": {

		// 一个ID为DelayLoad_Background_Soccer的群。
		"GameTrophies": {

			"ModOption": "BaseOn",
			// 修改策略，有三种取值，默认为OverWrite（不写出ModOption时的缺省值）。
			// PassIfExist 存在则跳过 ：若目标信息结构中存在这一资源群，将会跳过这一修改；若不存在，则会以下文数据构造一个新的资源群。
			// OverWrite 始终重写 ：若目标数据不存在这个群，则会以下文数据构造一个新的资源群。若存在，则会清空原数据中同名群的数据，并以下文数据构造同名群。
			// BaseOn 保留原数据基础 ：若目标数据不存在这个群，则会以下文数据构造一个新的资源群。若存在，则会在不改动原数据的情况下进行信息的添加。
			
			"IsComposite": true,
			// 是否为一个复合群。

			这个群所包含的子群。
			"SubGroup": {

				// 一个ID为GameTrophies_1536的子群。
				"GameTrophies_1536": {
					
					"ModOption": "BaseOn",
					// 修改策略，同上。

					"Category": [1536, null],
					// 分类属性，同上文中 RSB 结构 中说明的一致。

					// 这个子群所包含的资源文件。
					"Res": {

						// 这个资源的 ID 为 ATLASIMAGE_ATLAS_WORLDMAP_TROPHY_FM_1536 。
						"ATLASIMAGE_ATLAS_WORLDMAP_TROPHY_FM_1536": {

							"Path": [ "atlases", "WorldMap_Trophy_FM_1536" ],
							// 这个资源的路径，应将相应文件以这个路径放置于 Patch文件夹下的 Res文件夹 中，应用补丁时，程序将复制到目标数据包目录的 Res文件夹 中。

							"Type": "Image",
							// 文件的类型

							"AtlasInfo":{
								// 同上文提到的 AtlasInfo 一致，具体请看上文。

								"Size": [ 260, 502 ],

								// 这个Atlas所包含的子图像。
								"Image": {
									
									// ID 为 IMAGE_WORLDMAP_TROPHY_FM 的图像。
									"IMAGE_WORLDMAP_TROPHY_FM": {

										"Path": [ "images", "1536", "initial", "worldmap", "trophy_fm" ],
										"Type": "Image",

										"ImageInfo": [ 0, 0, 260, 502, null, null ]
										// 六个整数，分别为 X坐标、Y坐标、图像长度、图像宽度、游戏中偏移的X坐标、游戏中偏移的Y坐标。
										// 前四个为无符号整数，必须写明字面量；后两个为有符号整数（可为负数），并可用 空值 null 表示不偏移。

									}
								},
								"TexFmt": null
							}
						}
					}
				}
			}
		}
		
		// 一个ID为DelayLoad_Background_Soccer的群
		"DelayLoad_Background_Soccer": {

			"External": true
			// External 代表使用外部信息，即 ResInfo 与 StructInfo 保存在 Patch目录 下的 ResInfo 与 StructInfo 文件夹 中。
			// 使用 External 的修改都强制为 OverWrite 模式，即始终重写该群数据。
			// 这常用于快速从其他版本数据包内移植目标数据包所不存在的资源，可通过 RSB高级解包 功能提取分割后的 ResInfo 与 StructInfo ，直接将需要添加的资源群的 ResInfo 与 StructInfo 目录复制到 Patch文件夹 中相应子文件夹内。

		},

	}
}
```

4. 应用补丁，具体参考视频。