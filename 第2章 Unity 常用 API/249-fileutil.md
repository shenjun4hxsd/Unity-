##FileUtil



|函数|	返回类型|	解释|	注意|
|:--|:--|:--|:--|
|DeleteFileOrDirectory|bool|删除文件或文件夹|所有的文件目录分隔符必须使用“/”格式（Unix 风格）|
|CopyFileOrDirectory|void|复制文件或文件夹|一定要在函数的最后一个参数上添加要把文件拷贝到的目录|
|CopyFileOrDirectoryFollowSymlinks|void|复制文件或者文件夹|实际拷贝的是指定文件夹下的文件，并不包含这个指定的文件夹。|
|MoveFileOrDirectory|void|移动文件或文件夹||
|GetUniqueTempPathInProject|string|在项目获取唯一临时路径|	你可以说使用它创建临时文件或文件夹，并确保你没有覆盖其他的文件|
|ReplaceFile|void|替换文件|用src指定的文件替换Dst 指定的文件，如果dst指定的文件不存在这个方法会拷贝src指定的文件到dst，如果存在这个方法会下删除dst 指定的文件然后从scr指定的文件拷贝到dst。|
|ReplaceDirectory |void|替换文件夹|用src指定的文件夹替换Dst 指定的文件夹，如果dst指定的文件夹不存在这个方法会拷贝src指定的文件夹到dst文件夹，如果存在这个方法会下删除dst 指定的文件夹然后从scr指定的文件夹拷贝到dst指定的文件夹。|

