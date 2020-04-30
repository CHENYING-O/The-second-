# The-second-
2.0版本（基于第七组的基础开发第二版本，第七组部分内容如下）
项目：三角函数计算

功能说明：
输入一个角度，选择要计算的三角函数即可显示计算结果，输出结果保留三位小数。
输入不得大于2^20，否则显示"inf"，若计算结果不存在，同样显示“inf”；
点击“清零”按钮对输出进行清零。

关键文件组成：
动态链接库文件sincpp.dll，coscpp.dll，tancpp.dll，cotcpp.dll
头文件Trigonometry Calculator01Dlg.h定义控件变量，消息处理函数等
源文件Trigonometry Calculator01Dlg.cpp给出消息处理函数的实现


界面开发
进入资源视图->展开Dialog->双击IDD_TRIGONOMETRYCALCULATOR01_DIALOG即可管理界面
两个编辑框中已添加了两个double型变量m_editNUM和m_editResult，分别作为输入和输出
在源文件Trigonometry Calculator01Dlg.cpp中调用相应的动态链接库进行计算，
例如，点击"sin"按钮时，进入如下函数：
void CTrigonometryCalculator01Dlg::OnBnClickedSinButton()  //点击按钮sin，进入该函数
{
    UpdateData(TRUE);  // 将各控件中的数据保存到相应的变量
    typedef double(*lpSinFun)(double); //宏定义函数指针类型
    HINSTANCE hDll;   //DLL句柄 
    lpSinFun sinFun;  //函数指针
    hDll = LoadLibrary(_T("sincpp.dll"));  //动态获取dll文件路径
    m_editResult = 0; //初始化输出为零
    if (hDll != NULL)
    {
        sinFun = (lpSinFun)GetProcAddress(hDll, "sincpp");  //用sinFun取代dll库中的sincpp函数
        if (sinFun != NULL)
        {			
	m_editResult = sinFun(m_editNUM);  //计算sin
	UpdateData(FALSE);
        }
        FreeLibrary(hDll);  //释放dll
    }
}
