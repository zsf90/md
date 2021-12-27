# WPF 介绍



## 第一个 WPF 应用

用 Visual Studio 2022 创建一个 WPF项目，项目基于 .NET 6 框架。

**MainWindow.xaml**
```xml
<Window x:Class="WpfApp1.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:local="clr-namespace:WpfApp1"
        mc:Ignorable="d"
        Title="MainWindow" Height="450" Width="800">
    <Grid>
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="0*"/>
            <ColumnDefinition/>
        </Grid.ColumnDefinitions>

    </Grid>
</Window>
```

**MainWindow.xaml.cs**
```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows;
using System.Windows.Controls;
using System.Windows.Data;
using System.Windows.Documents;
using System.Windows.Input;
using System.Windows.Media;
using System.Windows.Media.Imaging;
using System.Windows.Navigation;
using System.Windows.Shapes;

namespace WpfApp1
{
    /// <summary>
    /// Interaction logic for MainWindow.xaml
    /// </summary>
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
        }
    }
}
```

**App.xaml**
```xml
<Application x:Class="WpfApp1.App"
             xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
             xmlns:local="clr-namespace:WpfApp1"
             StartupUri="MainWindow.xaml">
    <Application.Resources>
         
    </Application.Resources>
</Application>
```

## XAML 命名空间（x:）语言功能

## 

## 样式

我们可以创建多种样式，然后再多个控件上使用同样的样式。样式就相当于前端中的CSS样式表，使用了创建重复外观样式的技术。

下面是我们创建的样式代码，样式可以继承与其他样式。

```xml
<Window.Resources>
        <Style x:Key="ButtonBase" TargetType="Button">
            <Setter Property="FontSize" Value="20"></Setter>
            <Setter Property="Padding" Value="18"></Setter>
            <Setter Property="Margin" Value="4"></Setter>
            <Setter Property="Background" Value="Orange"></Setter>
            <Setter Property="Foreground" Value="White"></Setter>
        </Style>

        <!-- 继承自 ButtonBase 样式 --> 
        <Style x:Key="ButtonStyle1" TargetType="Button" BasedOn="{StaticResource ButtonBase}">
            <Setter Property="Width" Value="100"></Setter>
            <Setter Property="FontSize" Value="25"></Setter>
        </Style>
    </Window.Resources>
```

`ButtonStyle1` 样式继承与 `ButtonBase` 样式。



## 触发器

- Trigger: 简单触发器，该触发器检查特定的依赖的属性的值是否发生变化来使用Setter改变样式。
- MultiTrigger：类似于简单触发器，但是组合了多个条件，所有的条件必须被满足才会改变样式。
- DataTrigger：与简单触发器类似，该触发器与数据绑定一起运行。但是该触发器是检测任何绑定的数据是否发生变化。
- MultiDataTrigger：组合了多个DataTrigger。
- EventTrigger：这是最常用的触发器，当某个事件触发时来改变样式。

### Trigger 简单触发器

下面代码定义了一个**简单触发器**，当按钮的属性 'IsPressed' = True 时（鼠标按下），让按钮的字体大小为 30。

```xml
<!-- 继承自 ButtonBase 样式 --> 
<Style x:Key="ButtonStyle1" TargetType="Button" BasedOn="{StaticResource ButtonBase}">
    <!-- 定义一个简单触发器 -->
    <Style.Triggers>
        <!-- 当按钮的 IsPressed 为 True 时应用触发器 -->
        <Trigger Property="IsPressed" Value="True">
            <Setter Property="FontSize" Value="30"></Setter>
        </Trigger>
    </Style.Triggers>
    <Setter Property="Width" Value="100"></Setter>
    <Setter Property="FontSize" Value="25"></Setter>
</Style>
```

### MultiTrigger 多条件触发器

```xml
<!-- 继承自 ButtonBase 样式 --> 
<Style x:Key="ButtonStyle1" TargetType="Button" BasedOn="{StaticResource ButtonBase}">
    <!-- 定义一个触发器容器，里边可以放置多个触发器 -->
    <Style.Triggers>
        <!-- 一个简单触发器：当按钮的 IsMouseOver 为 True 时应用触发器。 -->
        <Trigger Property="IsMouseOver" Value="True">
            <Setter Property="Foreground" Value="Black"></Setter>
        </Trigger>
        <!-- 一个多条件触发器: 多条件触发器分为条件部分和设置部分。 -->
        <MultiTrigger>
            <MultiTrigger.Conditions>
                <Condition Property="IsPressed" Value="True"></Condition>
                <Condition Property="FontSize" Value="22"></Condition>
            </MultiTrigger.Conditions>
            <MultiTrigger.Setters>
                <Setter Property="Foreground" Value="Red"></Setter>
            </MultiTrigger.Setters>
        </MultiTrigger>
    </Style.Triggers>
    <Setter Property="Width" Value="100"></Setter>
    <Setter Property="FontSize" Value="22"></Setter>
</Style>
```

### DataTrigger 数据触发器

一下代码实现了在输入框中输入颜色名称后背景会随输入的颜色名称发生变化。

1. 首先将文本框的**Text**属性绑定到自己的**Background**属性上。
2. 将数据触发器绑定上输入框里文本的长度，当等于大于 20 时，就会执行数据触发器里的内容。

```xml
<!-- 数据触发器 -->
<Style x:Key="DataStyleTrigger" TargetType="TextBox">
    <Setter Property="Background" Value="{Binding RelativeSource={RelativeSource self}, Path=Text}"></Setter>
    <Style.Triggers>
        <DataTrigger Binding="{Binding RelativeSource={RelativeSource self}, Path=Text.Length}" Value="20">
            <Setter Property="IsEnabled" Value="False"></Setter>
        </DataTrigger>
    </Style.Triggers>
</Style>
....
<StackPanel Grid.Row="1" Grid.Column="1">
    <TextBox Name="ColorText" Style="{StaticResource DataStyleTrigger}" Padding="10" Margin="20 10 20 5"></TextBox>
</StackPanel>
```

### MultiDataTrigger 多条件数据触发器

```xml
<Style x:Key="DataTextTrigger-Multi">
    <Setter Property="ItemsControl.FontSize" Value="20"></Setter>
    <Setter Property="ItemsControl.Margin"  Value="10"></Setter>
    <Style.Triggers>
        <MultiDataTrigger>
            <MultiDataTrigger.Conditions>
                <Condition Binding="{Binding ElementName=cb1, Path=IsChecked}" Value="True"></Condition>
                <Condition Binding="{Binding ElementName=cb2, Path=IsChecked}" Value="True"></Condition>
            </MultiDataTrigger.Conditions>
            <MultiDataTrigger.Setters>
                <Setter Property="ItemsControl.Background" Value="Purple"></Setter>
            </MultiDataTrigger.Setters>
        </MultiDataTrigger>
    </Style.Triggers>
</Style>

<StackPanel Grid.Row="2" Grid.Column="1" Style="{StaticResource DataTextTrigger-Multi}">
    <CheckBox Name="cb1">全选背景变为紫色</CheckBox>
    <CheckBox Name="cb2">全选背景变为紫色</CheckBox>
</StackPanel>
```

## 控件模板

在**WPF**中，每个控件都有一个默认的控件模板，用于定义控件的基本外观和行为。**WPF**使用**ControlTemplate**定义控件的外观，每个控件都有一个**Template**的属性。默认情况下。控件通过该属性获取呈现的外观，通过更改**ControlTemplate**，可以为控件定义一个新的样式。

```xml
<Window x:Class="WPFDemo.TemplateDemo"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:local="clr-namespace:WPFDemo"
        mc:Ignorable="d"
        Title="TemplateDemo" Height="300" Width="300">
    <Window.Resources>
        <ControlTemplate x:Key="ButtomTemplate" TargetType="{x:Type Button}">
 
            <Border Name="Border" BorderBrush="{TemplateBinding Foreground}"
                    BorderThickness="{TemplateBinding BorderThickness}"
                    CornerRadius="5" Background="{TemplateBinding Background}"
                    TextBlock.Foreground="{TemplateBinding Foreground}"
                    Margin="{TemplateBinding Margin}">
                <ContentPresenter RecognizesAccessKey="True"
                                  HorizontalAlignment="{TemplateBinding HorizontalAlignment}"
                                  VerticalAlignment="{TemplateBinding VerticalAlignment}">
 
                </ContentPresenter>
            </Border>
            <!--设置模板触发器-->
            <ControlTemplate.Triggers>
                <Trigger Property="IsMouseOver"  Value="True">
                    <Setter TargetName="Border" Property="Background" Value="Red"></Setter>
                </Trigger>
                <Trigger Property="IsPressed" Value="True">
                    <Setter TargetName="Border" Property="Background" Value="Yellow"></Setter>
                </Trigger>
            </ControlTemplate.Triggers>
        </ControlTemplate>
    </Window.Resources>
    <Grid>
 
        <Button   Template="{StaticResource ButtomTemplate}" Margin="10" Background="LightBlue" Content="我是套用模板的button"></Button>
    </Grid>
</Window>
```



