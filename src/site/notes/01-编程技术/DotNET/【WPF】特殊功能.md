---
{"dg-publish":true,"permalink":"/01-编程技术/DotNET/【WPF】特殊功能/","dgPassFrontmatter":true,"created":"2023-10-27T09:00:33.420+08:00","updated":"2024-01-19T08:47:56.000+08:00"}
---

#wpf 

# 打开内嵌浏览器

``` C#
private void Grid_Loaded(object sender, RoutedEventArgs e)
{
	WebBrowser webBrowser = new WebBrowser();
	webBrowser.Source = new Uri("https://qiniu.bigdudu.cn/比对软件使用说明.pdf");
	this.Content = webBrowser;
}
```

# 浏览并选择文件夹

``` C#
System.Windows.Forms.FolderBrowserDialog openFileDialog = new System.Windows.Forms.FolderBrowserDialog();  //选择文件夹

if (openFileDialog.ShowDialog() == System.Windows.Forms.DialogResult.OK)//注意，此处一定要手动引入System.Window.Forms空间，否则你如果使用默认的DialogResult会发现没有OK属性
{

	vm.BaseFolderPath = openFileDialog.SelectedPath;
}
```