---
description: V-DevSuite对默认通用类规则、函数、控件插件进行源码公开
---

# 源码公开(代码)

## 开源协议

基于LGPL（GNU Lesser General Public License）

## 源码地址

[前端规则](https://github.com/opensource-vplatform/vplatform-plugin-rule-client)

[前端函数](https://github.com/opensource-vplatform/vplatform-plugin-function-client)

[前端控件](https://github.com/opensource-vplatform/vplatform-plugin-widget-smartclient)

[后端规则](https://github.com/opensource-vplatform/vplatform-plugin-rule-server)

[后端函数](https://github.com/opensource-vplatform/vplatform-plugin-function-server)

_注：release branch为正式版，main branch为开发版_

****[**平台开发的测试代码**](https://github.com/opensource-vplatform/vplatform-plugin-example.git)** **

## 开源进度_（更新时间2021-08-29）_

### 1. 已开源的插件

{% tabs %}
{% tab title="后端规则" %}
SetEntityVarControlValue--给界面实体/控件/变量赋值,AbortRule--中断规则,AddTableRecord--新增实体记录,SetLoopVariant--给循环变量赋值,ExecExpression--执行函数表达式,ClearEntityData--清除实体记录,ExceptionAbort--异常中断,AbortLoop--中断循环,DeleteConditionRelationData-删除数据库中的记录,EntityConditionRemove-删除实体记录,EntityRecordRecycling-实体记录循环处理,GenerateXMLOrJSON-配置数据生成,UpdateRecord-保存实体到数据,CallWebApi-调用webapi,CallWebService-调用webservice,ServerRestoreXMLOrJSON-配置数据还原,DataBaseDataToInterfaceEntity-从数据库获取数据到实体,ModifyDataBaseRecord-修改数据库中的记录,ExecuteVoidQuery-执行无返回值的查询,ImportProjectToDBOrEntity-Project导入到数据库表或实体,DataBaseDataToInterfaceEntity-从数据库获取数据到实体,CopyRecordBetweenEntity-实体间复制记录,ServPrintDataTrans-报表打印数据转换,CopyRecordBetweenTables-表间数据复制,ExportDBOrEntityDataToExcel-导出数据库或实体数据到Excel,ImportExcelToDBOrEntity--Excel导入到数据库表或实体,Serverrule\_ExecuteRuleSet-执行方法
{% endtab %}

{% tab title="后端函数" %}
ABS,Acos,AddThousandBits,AsciiToUnicode,Asin,Atan,AvgColumnFunc,Ceiling,ChangeMoneyToChinese,CheckCertCodeFun,CheckChinese,ChineseToUnicode,Compare,ConcatStr,Contains,ConvertFunc,ConvertPageNumber,ConvertVarToEntityColumn,CopyFileByFileId,Cos,Cosh,DateAddFunc,DateConvert,Datediff,DateSub,DateTimeNow,DateTimeToUnixtimestamp,DateToString,DecodeBASE64,DecodeURIComponent,DecryptFunc,Divrem,DowdloadFileToFileSystem,DownloadFromFtp,E,EncodeBASE64,EncodeURIComponent,EncryptFunc,EncryptionFunc,EndsWith,EvalExpression,Exp,Floor,Format,GenerateUUID,GetAgeByIdCard,GetAllParentIds,GetConditionColumnValue,GetDateSection,GetDistance,GetEntityRowCountFunc,GetFileInfo,GetFileUrlByFileId,GetFirstRowColumnValue,GetImageUrl,GetLength,Log10,Max,LogFunc,Min,Pai,Pow,Sin,Sinh,Tan,Tanh,UnixtimestampToDateTime,TrimEnd,TrimStart,IsWhiteOrSpace,Insert,GetIPAddressFunc,UnicodeToChinese,UnicodeToAscii,IsEmpty,GetSystemVariable,IsNullOrEmptyFunc,ListToStringFunc,PadRight,ServerAdd,Random,RecyclingSequenceNumber,Remainder,Remove,ReplaceByIndex,ReplaceFunc,Round,ServerDivide,ServerMultiply,ServerSubtract,ShortDateNow,ShortTimeNow,Sign,StandardMD5Encrypt,StartsWith,Substring,ToLower,TotalColumnFunc,ToUpper,TreeDataUpwardCollect,Truncate,UploadToFtp,V3If,VConvertEntityToJsonFunc,VConvertEntityToXMLFunc,VRestoreJsonToEntityFunc,VRestoreXMLToEntityFunc,LastIndexOf,Null,MaxColumn,NumberCodeAdd,PadLeft,MD5EncryptByUTF8,GetProperty,GenerateSequenceNumber,GenerateSequenceNumberQuick,GetDataBaseType,GetLngOrLatByAddr,GetLoactionPlace,GetSerialNumberFunc,GetTableData,GetZoomLevel,HasRecordFunc,IndexOf,IsNull,MD5Encrypt,MinColumn,Sqrt
{% endtab %}

{% tab title="前端规则" %}
AbortLoop--中断循环,AbortRule--中断规则,AddTableRecord-新增实体记录,Webrule\_ShowMessageWhenChanged--判断指定实体的数据是否发生变化,Webrule\_ShowMessage--显示设设置提示信息,Webrule\_SetLoopVariant--给循环变量赋值,Webrule\_SetControlPropertys--控件属性设置,Webrule\_SelectionConfirm--退出窗体,Webrule\_ResetSelectedControlValue--清空控件数值,Webrule\_RefreshSystemVariable--刷新构建变量的值,Webrule\_Progress--进度条显示隐藏,Webrule\_MakeControlRVE--控制控件的只读、使能、显示,Webrule\_LocateSelectRecord--记录选中/取消选中,LocateCurrentRecord--记录定位,PageVisibleControl--页签显示控制,TreeNodeMoveUpDownEditor--实体树形操作,RefreshControlItems--刷新控件的候选项,PrintOperation--打印及预览操作,TreeGridStatistics-树表数值字段向上汇总,TreeDisplayOper--树节点展开折叠,UpdateRecord--保存实体到数据库,Webrule\_CalculateToColumn--计算公式的值并赋值给指定字段,Webrule\_ClearInterfaceEntityData--清除界面实体中的记录,Webrule\_CopyTreeDataBetweenEntities--树形结构实体间数据复制,Webrule\_CursorJumpControl--光标跳转控制,Webrule\_DeleteListSelectRow--删除实体记录,SetEntityVarControlValue--给界面实体/控件/变量赋值,AddDataBaseRecord--新增数据库记录,InterfaceEntityRecordRecycling---界面实体记录循环处理,MakeCertPic--生成数据验证码,ModifyDataBaseRecord--修改数据库中的记录,RecordReferenceCheck--记录引用检查,RestoreXMLOrJSON--配置数据还原,VucRedirector--统一认证跳转,GetRecordCount--获取数据库表中的记录,I18nOperation--多语言操作,ImportExcelToDB--Excel导入到数据库表,JGStepsOperation 步骤条操作,Webrule\_ExportDataToExcel--导出数据库数据到Excel,Webrule\_DataValidationEditor--数据合法性校验,Webrule\_CopyEntityRecord--实体间复制记录,Webrule\_CompareEntityData--界面实体之间数据比较,Webrule\_CheckUnique--前后台唯一性检查,Webrule\_CheckCertCode--校验验证码,Webrule\_CancelCloseWindow--取消窗体关闭,Webrule\_Attachmentoperation--附件操作,Webrule\_GenerateXMLOrJSON--配置数据生成,Webrule\_HardwareOperation--移动设备硬件操作,Webrule\_GetLocalDBToEntity--从手机数据库获取数据到实体,Webrule\_SaveFileToAlbum--保存图片或视频到相册,Webrule\_SaveFile--保存图片,Webrule\_SaveEntityToLocalDB--保存实体到手机数据库,单据编号生成（Webrule\_GenerateBillCode）,导出表格数据到Excel（Webrule\_ExportTableData）,执行函数/表达式（Webrule\_ExecExpression）,加载动态交叉表到实体（Webrule\_DynamicCrossDataToInterfaceEntity）,将表（及从表）数据插入到其它表（Webrule\_DocumentsConverted）,删除数据库中的记录（Webrule\_DeleteConditionRelationData）,从数据库获取数据到实体（Webrule\_DataBaseDataToInterfaceEntity）,树形结构表间数据复制（Webrule\_CopyTreeDataBetweenTables）,表间数据复制（Webrule\_CopyDataBetweenTables）,界面实体与物理表数据比较（Webrule\_CompareEntityAndTableData）,调用WebAPI（Webrule\_CallWebApi）,计算公式的值并赋值给指定字段（Webrule\_CalculateToColumn）,获取当前位置的经纬度（Webrule\_GetCurrentPosition）,获取与指定经纬度最接近的经纬度（Webrule\_GetClosestPosition）,获取当前App版本号（Webrule\_CheckAppVersion）,OpenComponentReturnData-打开窗体并返回数据,OpenLink-打开链接地址,从数据库或实体获取数据到报表（Webrule\_DataBaseDataToReport）
{% endtab %}

{% tab title="前端函数" %}
Acos,Add,AsciiToUnicode,Asin,Atan,BigMul,Ceiling,ChineseToUnicode,Compare,ConcatStr,Contains,ConvertAbsoluteValue,Cos,Cosh,DateAdd,DateConvert,DateSub,DateTimeNow,DateTimeToUnixtimestamp,DateToString,Datediff,Divide,Divrem,E,EndsWith,Exp,Floor,Format,GetDateSection,GetLength,IndexOf,Insert,LastIndexOf,LocateDateTimeNow,Log,Log10,Max,Min,Multiply,PadLeft,PadRight,Pai,Pow,Random,Remainder,Remove,Replace,ReplaceByIndex,Round,ShortDateNow,ShortTimeNow,Sign,Sin,Sinh,Sqrt,StartsWith,Substring,Subtract,Tan,Tanh,ToLower,ToUpper,Trim,TrimEnd,TrimStart,Truncate,UnicodeToAscii,UnicodeToChinese,UnixtimestampToDateTime,GetAgeByIdCard,AvgColumn,ChangeMoneyToChinese,CheckChinese,Convert,ConvertUnit,ConvertVarToEntityColumn,DateTimeToChinese,DecodeURIComponent,EncodeURIComponent,EntityToVar,EvalExpression,GetAgeByIdCard,GetBirthdayByIdCard,GetBrowserInfo,GetCurrentRecordIndex,GetEntityCurrentColumnValue,GetEntityRowCount,GetEntitySelectedRowCount,GetFirstRowColumnValue,GetLocalStorage,GetRowNum,GetSystemCurrentTimeMillis,IsNull,IsNullOrEmpty,ListToString,MaxColumn,MinColumn,NumiceToChinese,PrintToBrowserConsole,SetLocalStorage,SetRowNum,TotalColumn,V3If,VarToEntity,FindControl,GenerateSequenceNumberQuick,GenerateSequenceNumber,GetHost,GetImageUrlByFileId,GetIPAddress,GetRequestParmByKey,GetSelectedDateToEntity,GetSerialNumber,GetTableData,GetTreeEntityIds,GetWidgetSize,GetWindowCode,HasRecord,IsExistWindowInstanceCode,IsLeaf,IsLogin,IsSelectedRecord,MD5Encrypt,MD5EncryptByUTF8,Null,NumberCodeAdd,RecyclingSequenceNumber,SelectedDateFromEntity ,SelectOrQuitAllRecords,SetControlProperty,setCookie,SetCurrentWindowTitle,SetGridColReadOnly,SetGridColTitle,SetGridColVisble,SetWindowState,StandardMD5Encrypt,TeeNodePath,VConvertEntityToJson,VConvertKeyValueEntityToJson,VConvertEntityToXML,VRestoreJsonToEntity,DecryptFunc,DeleteCookie 1,DeleteFileByFileId,EncryptFunc,GenerateTextByTemplate,GenerateUUID,GetChartClickData,GetComponentCode,GetContextPath,GetControlProperty,GetCookie 12,GetCurrentCalendarMonth,GetCurrentWindowInstanceCode,GetDataBaseType,GetDynamicColumnIdentity,GetFileInfo,GetGridComp,VRestoreXMLToEntity,ScrollToPosition,SetToolBarText
{% endtab %}

{% tab title="前端控件" %}
JGButton,JGImage,JGTimer,JGHyperLink,JGCheckBox,JGFloatBox,JGLongTextBox,JGPassword,JGTextBox,JGBaseDictBox,JGComboBox,JGDateTimePicker,JGIntegerBox,JGCheckBoxGroup,JGRadioGroup,JGLongDateTimePicker,JGPeriod,JGPercent,JGLinkLabel--链接标签,JGLabel--标签,JGSteps--步骤条,JGTabContro--页签控件,JGAttachment--文件,JGChart-图表,JGCalendar--日历,JGRecordPaging--记录导航控件,JGWebBrowser--网页控件,JGLocateBox--检索控件,JGNewsList--新闻列表控件,JGGroupPanel--排列,JGReport--报表控件,JGRichTextEditor-富文本,JGButtonGroup--按钮组, JGStartMenu-开始菜单,JGDiv-自定义窗体,JGNavigator-导航菜单,JGActivityPanel活动面板,JGPropertyEditor属性编辑器,JGPortal门户,JGImagePlay图片播放,RichTextViewer富文本浏览
{% endtab %}
{% endtabs %}

### 2. 测试验证中的插件

{% tabs %}
{% tab title="后端规则" %}
已完成
{% endtab %}

{% tab title="后端函数" %}
已完成
{% endtab %}

{% tab title="前端规则" %}
已完成
{% endtab %}

{% tab title="前端函数" %}
已完成
{% endtab %}

{% tab title="前端控件" %}
已完成
{% endtab %}
{% endtabs %}

### 3. 准备开源的插件

{% tabs %}
{% tab title="后端规则" %}
已完成
{% endtab %}

{% tab title="后端函数" %}
已完成
{% endtab %}

{% tab title="前端规则" %}
已完成
{% endtab %}

{% tab title="前端函数" %}
已完成
{% endtab %}

{% tab title="前端控件" %}
JGDataGrid,JGTreeGrid,JGTreeView
{% endtab %}
{% endtabs %}
