---
title: MFC单文档中添加按钮控件和消息响应函数
date: 2015-12-09 19:42
tags: MFC
categories: 桌面开发
---

-----
详细见：http://blog.csdn.net/lwq573384928/article/details/50240455
<!--more-->
----
通过以下三步即可完成
# 创建动态控件
两种方法：
1. 在资源管理器视图中的Resource.h头文件中自定义

如：#defineID_DemoButton  123456
2. 资源视图的String Table中定义

在最下端双击新建

# 定义并显示按钮
在View类声明成员变量CButton demoButton，用向导类工具在C***View（***指工程名）中添加OnCreate 或OnInitialUpdate 函数


在函数中加入按钮定义
       CRectrect_button(40,10,200,60);   //控制按钮大小、位置
       m_button.Create("Button1",WS_CHILD|WS_VISIBLE|WS_BORDER,rect_button,this, ID_DemoButton);
       m_button.ShowWindow(SW_SHOWNORMAL);
注：Create方法的最后一个参数为第一步中定义的按键资源的名称
这样按钮应该都可以显示出来了。

附：
不同种类的控件应创建不同的类对象，
按钮控件 CButton （包括普通按钮、单选按钮和复选按钮）
编辑控件 CEdit
静态文本控件 CStatic
标签控件 CTabCtrl
旋转控件 CSpinButtonCtrl
滑标控件 CSliderCtrl
多信息编辑控件 CRichEditCtrl
进度条控件 CProgressCtrl
滚动条控件 CSrcollBar
组合框控件 CComboBox
列表框控件 CListBox
图像列表控件 CImageCtrl
树状控件 CTreeCtrl
动画控件 CAnimateCtrl
本例中我们创建一个CButton类的普通按钮。注意不能直接定义CButton对象，如：CButton m_MyBut;这种定义只能用来给静态控件定义控制变量，不能用于动态控件。
正确做法是用new调用CButton构造函数生成一个实例：
CButton *p_MyBut = new CButton();
然后用CButton类的Create()函数创建，该函数原型如下：
BOOL Create( LPCTSTR lpszCaption, DWORD dwStyle, const RECT& rect, CWnd*pParentWnd, UINT nID );
lpszCaption是按钮上显示的文本；
dwStyle指定按钮风格，可以是按钮风格与窗口风格的组合，取值有：
窗口风格：
WS_CHILD 子窗口，必须有
WS_VISIBLE 窗口可见，一般都有
WS_DISABLED 禁用窗口，创建初始状态为灰色不可用的按钮时使用
WS_TABSTOP 可用Tab键选择
WS_GROUP 成组，用于成组的单选按钮中的第一个按钮
按钮风格：
BS_PUSHBUTTON 下压式按钮，也即普通按钮
BS_AUTORADIOBUTTON 含自动选中状态的单选按钮
BS_RADIOBUTTON 单选按钮，不常用
BS_AUTOCHECKBOX 含自动选中状态的复选按钮
BS_CHECKBOX 复选按钮，不常用
BS_AUTO3STATE 含自动选中状态的三态复选按钮
BS_3STATE 三态复选按钮，不常用
以上风格指定了创建的按钮类型，不能同时使用，但必须有其一。
BS_BITMAP 按钮上将显示位图
BS_DEFPUSHBUTTON 设置为默认按钮，只用于下压式按钮，一个对话框中只能指定一个默认按钮
rect指定按钮的大小和位置；
pParentWnd指示拥有按钮的父窗口，不能为NULL；
nID指定与按钮关联的ID号，用上一步创建的ID号。
不同控件类的Create()函数略有不同，可参考相关资料。
例：p_MyBut->Create("动态按钮", WS_CHILD | WS_VISIBLE | BS_PUSHBUTTON,CRect(20,10,80,40), this, IDC_MYBUTTON );
这样，我们就在当前对话框中的(20,10)处创建了宽60，高30，按钮文字为“动态按钮”的下压式按钮。

# 添加消息响应函数
1. C***View头文件中MESSAGE_MAP中添加响应函数：afx_msg void OnBtnDown();

C***View.cpp文件的BEGIN_MESSAGE_MAP和  END_MESSAGE_MAP 之间加入ON_BN_CLICKED(ID_DemoButton,OnBtnDown) 用来关联按钮和函数；
2. 加入按键响应函数的定义
```c++
void C***View::OnBtnDown()
{
         MessageBox("hello","helloworld",MB_OK);//按自己的功能需求写
}
```
除了按钮的响应函数外，你还可以用上面获得的指针访问按钮，如：
修改按钮的大小和位置：p_MyBut[0]->MoveWindow(……);
修改按钮文本：p_MyBut[0]->SetWindowText(……);
显示/隐藏按钮：p_MyBut[0]->ShowWindow(……);