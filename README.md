# A_M_audit9.0
将A_M_Audit修改以适配IDA 9.0及以上版本
# A_M_audit修改

修改A_M_audit.py以便于在IDA Pro 9.0中使用
IDA 9.0删除了函数get_inf_structure
在IDA9.0运行脚本会报错
我们修改A_M_audit.py
在头部添加
```py
import ida_ida
```
搜索info = idaapi._get_inf_structure()
将以下代码
```py
    def run(self, arg):
        '''
                    每次运行插件时, 执行的具体操作
                    功能代码在此编写
        '''

        info = idaapi.get_inf_structure()
        print(info.procName)
        if 'mips' in info.procName:
            m_mips=CMipsAudit()
            m_mips.MipsAudit()
        elif 'ARM' in info.procName:
            m_arm=CArmAudit()
            m_arm.ArmAudit()
        else:
            print('A_M_Audit is not supported on the current arch')
```
修改为
```py
def run(self, arg):
        '''
                    每次运行插件时, 执行的具体操作
                    功能代码在此编写
        '''

        #info = idaapi.get_inf_structure()
        print(ida_ida.inf_get_procname())
        is_ida = ida_ida.inf_get_procname()
        if 'mips' in is_ida:
            m_mips=CMipsAudit()
            m_mips.MipsAudit()
        elif 'ARM' in is_ida:
            m_arm=CArmAudit()
            m_arm.ArmAudit()
        else:
            print('A_M_Audit is not supported on the current arch')
```
