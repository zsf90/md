# Panel 控件

## 通过按钮控制 Panel 是否显示

```csharp
private void button1_Click(object sender, EventArgs e)
{
    if(panel1.Visible)
    {
        panel1.Hide();
    } else
    {
        panel1.Show();
    }
}
```
