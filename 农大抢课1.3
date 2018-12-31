import requests
from lxml import etree
import time
from PIL import Image
import csv
import threading
import sys
import os
import re

__version__ = "1.3"
__author__ = "迟我行"
__change__="有以下改动哦:\n1.更改了检验机制2.导出信息更加详细"
Choosing_Kc_List = []
willSelectClass = []
session = requests.session()
selectedList=[]
flag_dict ={}

headers = {
'Upgrade-Insecure-Requests': '1',
'User-Agent': 'Mozilla/5.0 (Linux; Android 6.0; Nexus 5 Build/MRA58N) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/70.0.3538.77 Mobile Safari/537.36',
}
url = 'http://jw.sdau.edu.cn/loginAction.do'
xuankeUrl = 'http://jw.sdau.edu.cn/xkAction.do?xkoper=wsxk'
chengjiUrl = 'http://jw.sdau.edu.cn/gradeLnAllAction.do?type=ln&oper=qbinfo&lnxndm=2018-2019%E5%AD%A6%E5%B9%B4%E7%A7%8B(%E4%B8%A4%E5%AD%A6%E6%9C%9F)#qb_2018-2019%E5%AD%A6%E5%B9%B4%E7%A7%8B(%E4%B8%A4%E5%AD%A6%E6%9C%9F)'
xkurl = 'http://jw.sdau.edu.cn/xkAction.do?actionType=3&pageNumber='
xuckurl = 'http://jw.sdau.edu.cn/xkAction.do?actionType=-1&fajhh=1655'
chooseurl = 'http://jw.sdau.edu.cn/xkAction.do'
selectedClasses = 'http://jw.sdau.edu.cn/xkAction.do?actionType=6'

def getyzm():
    global session
    try:
        imgres = session.get('http://jw.sdau.edu.cn/validateCodeAction.do?random=0.32352508289837867',timeout = 10)
        with open('yzm.jpg', 'wb') as imgfile:
            imgfile.write(imgres.content)
        time.sleep(4)
        img = Image.open('yzm.jpg')
        img.show()
    except:
        print('\nerror:请接通学校内网再运行程序..')
        sys.exit()


def loginPro():
    global session
    threading.Thread(target=getyzm).start()
    time.sleep(5)
    userid = input('请输入学号:')
    password = input('请输入密码:')
    yzm = input('请输入验证码(验证码位于文件的目录下):')
    info = "正在登陆,请稍后."
    for i in range(0,5):
        info = info + '.'
        time.sleep(0.8)
        print(("\r" + info).format(i), end="")
    data = {
        'zjh1': '',
        'tips': '',
        'lx': '',
        'evalue': '',
        'eflag': '',
        'fs': '',
        'dzslh': '',
        'zjh': userid,
        'mm': password,
        'v_yzm': yzm
    }
    try:
        res = session.post(url=url, headers=headers, data=data)
        html = etree.HTML(res.text)
        title =  html.xpath("//title/text()")[0]
        if title =="学分制综合教务":
            print("\n登陆成功！")
            return session
        else:
            print("\n登录失败!请重新输入")
            return loginPro()
    except:
        print('error:网络错误,原因未知！')
        sys.exit()


def getAvaClass():
    global session
    classList = []
    session.get(url = xuckurl)
    for i in range(1,13):
        res = session.get(url=xkurl+str(i))
        html = etree.HTML(res.text)
        pageNo = html.xpath("//form[@name='xkActionFormA']//td/text()")[0]
        classhtml = html.xpath("//form[@name='xkActionForm']//table[@class='displayTag']//tr[contains(@class,'odd') or contains(@class,'even')]")
        pageNo = re.match(".*?第(\d+)页", pageNo, re.S).group(1)
        for i, item in enumerate(classhtml):
            classes = {}
            list = item.xpath(".//td")
            if len(list) > 8:
                classes['kcxf'] = ''.join(list[5].text.split())  # 课程学分
                classes['kcid'] = ''.join(list[2].text.split())  # 课程id
                classes['kcxh'] = ''.join(list[4].text.split())  # 课程序号
                classes['kcxx'] = ''.join(list[17].text.split())  # 课程学校
                classes['kczc'] = ''.join(list[13].text.split())  # 课程周次
                classes['kcxq'] = ''.join(list[14].text.split())  # 课程星期
                classes['kcjc'] = ''.join(list[15].text.split())  # 课程节次
                if not list[3].xpath(".//span") == []:
                    classes['kcm'] = ''.join(list[3].xpath(".//span")[0].text).split()  # 课程名
                else:
                    classes['kcm'] =''.join(list[3].xpath('.//a')[0].text).split()
                classes['teacher'] = ''.join(list[8].text).split()  # 课程教师
                classes["kcyh"] = pageNo  # 课程页号
                classList.append(classes)
            else:
                classList[-1]['kcxq'] = classList[-1]['kcxq'] + '\n' + ''.join(list[1].text.split())
                classList[-1]['kcjc'] = classList[-1]['kcjc'] + '\n' + ''.join(list[2].text.split())
    return classList

def saveAsFile(classList,i):
    if i==1:
        name = '可选课程'
        title=['课程号', '课程名', '课程序号', '页号','课学分', '教师','周次','星期','节次','校区']
        with open(name + '.csv', 'w+', encoding='utf-8-sig') as file:
            writer = csv.writer(file)
            writer.writerow(title)
            for item in classList:
                writer.writerow([item['kcid'], item['kcm'], item['kcxh'],item['kcyh'], item['kcxf'], item['teacher'],item['kczc'],
                                 item['kcxq'],item['kcjc'],item['kcxx']])
    else:
        name = '已选课程'
        title = ['课程号','课程名','课程序号','课学分','教师','校区']
        with open(name + '.csv', 'w+', encoding='utf-8-sig') as file:
            writer = csv.writer(file)
            writer.writerow(title)
            for item in classList:
                writer.writerow([item['kcid'], item['kcm'], item['kcxh'], item['xf'], item['teacher'], item['kcxx']])

    print('提示: ---------已经导出到:',os.path.abspath(__file__),'\\',name,'.csv---------\n')
    time.sleep(5)

def qk(kcid,pageNum):
    global Choosing_Kc_List
    global session
    global selectedList
    global flag_dict
    info3 = {
        'pageNumber1': pageNum,
        'actionType': 3,
    }
    session.get(url=xuankeUrl)  # 打开选课界面
    session.get(url='http://jw.sdau.edu.cn/xkAction.do?actionType=-1&fajhh=1655')  # 勾选1655
    session.get(url='http://jw.sdau.edu.cn/xkAction.do?actionType=2&pageNumber=-1&oper1=ori')  # 打开校任选课
    session.get(url='http://jw.sdau.edu.cn/xkAction.do?actionType=3&pageNumber=-1')
    session.post(url=chooseurl, data=info3)  # 跳转第七页
    while(1):
        if flag_dict[kcid] ==1:
            try:
                datas = {
                    'kcId': kcid,
                    'preActionType': '3',
                    'actionType': '9',
                }
                session.post(url=chooseurl, data=datas,timeout=20)
                time.sleep(2)
            except:
                print('错误:网络异常，2秒后关闭抢课线程')
                for item in Choosing_Kc_List:
                    if item['kcid'] == kcid:
                        Choosing_Kc_List.remove(item)
                time.sleep(2)
                sys.exit()
        else:
            print("已经抢到",len(selectedList),"门课")
            sys.exit()

def getselectedClasses():
    global session
    res = session.get(url=selectedClasses)
    html = etree.HTML(res.text)
    list = html.xpath("//tr[@class = 'odd']")
    selectedClassesList = []
    for item in list:
        clas = {}
        l = item.xpath(".//td")
        if len(l) > 10:
            kcid = l[1].text
            clas['kcid'] = ''.join(kcid.split())
            kcm = l[2].text
            clas['kcm'] = ''.join(kcm.split())
            xf = l[4].text
            clas['xf'] = ''.join(xf.split())
            teacher = l[7].text
            clas['teacher'] = ''.join(teacher.split())
            kcxx = l[3].text
            clas['kcxh'] = ''.join(kcxx.split())
            clas['kcxx'] = ''.join(l[15].text.split())
            selectedClassesList.append(clas)
    return selectedClassesList

def checkFin():
    global session
    global Choosing_Kc_List
    global selectedList
    global flag_dict
    while(1):
        if len(Choosing_Kc_List)>0:
            List = getselectedClasses()
            time.sleep(5)
            for item in List:
                for id in Choosing_Kc_List:
                    if item['kcid'] == id['kcid'][0:-3]:
                        Choosing_Kc_List.remove(id)
                        selectedList.append(id)
                        flag_dict[id['kcid']] = 0

def inputInf():
    global Choosing_Kc_List
    global session
    global flag_dict
    kcidList=[]
    while (1):
        kc = {}
        kc['kcid'] = input('--请输入所选择的课程号--格式:课程号+下划线+课序号,如:XT108001_05: ')
        kc['pageNum'] = input('--请输入课程所在的选课页面的页号: ')
        kcidList.append(kc)
        Choosing_Kc_List.append(kc)
        flag_dict[kc['kcid']] = 1
        print('已输入',len(Choosing_Kc_List),'门课程。')
        flagE = input('继续输入吗? (继续请输入y/结束请输入n)')
        if flagE == 'n'or flagE =='N':
            break
        elif flagE =='y' or flagE=='Y':
            pass
        else:
            break
    for i,item in enumerate(kcidList):
        threading.Thread(target=qk,args=(item['kcid'],item['pageNum']),daemon=True).start()
        time.sleep(2)
    print("·······抢课已在新线程的后台开启，正在抢", len(Choosing_Kc_List), "门课··········\n")
    time.sleep(3)

def showInfo():
    info = "当前正在抢"+str(threading.active_count)+"门课，请不要不要关闭程序"
    i=0
    while(1):
        while not len(Choosing_Kc_List)==0:
            info = info + '.'
            i+=1
            time.sleep(0.4)
            print(("\r "+info).format(i) ,end="")
            if i==7:
                i=0
                info = "当前正在抢"+str(threading.active_count()-2)+"门课，请不要不要关闭程序"

def option():
    global session
    global Choosing_Kc_List
    while 1:
        flag = input("\n"
                     "|---------------------------------------------------------------------    \n"
                     "|                                                                     |\n"
                     "|---输入1---进行抢课                                                  |  \n"
                     "|---输入2---查看导出所有可选课程信息到本地                            |  \n"
                     "|---输入3---查看并导出已选取所有课程信息         author:迟我行        |    \n"
                     "|---输入4---查看课程是否抢到                                          |     \n"
                     "|---输入exit---退出当前系统                                           |      \n"
                     "|                                                                     |\n"
                     "|---------------------------------------------------------------------|\n"
                     )
        if flag == '1':
            inputInf()
        elif flag == '2':
            if len(Choosing_Kc_List)==0:
                saveAsFile(getAvaClass(),i=1)
            else:
                print("抢课时不能进行导出操作哦~")
                time.sleep(3)
        elif flag  == '3':
            saveAsFile(getselectedClasses(),i=0)
        elif flag =="4" :
            print("正在查看请稍后..")
            if len(Choosing_Kc_List) >0:
                print("选课还在进行呦,您还有",len(Choosing_Kc_List),"课没选上哦~~")
                time.sleep(5)
            else:
                print("您还未进行抢课!请先开启抢课哦~")
                time.sleep(5)
        elif flag =="exit":
            sys.exit()
        elif '作者' in flag:
            print("Author:__高欣建__")
            time.sleep(10)
        elif '微信' in flag:
            print("wechat:youngyoungago")
            time.sleep(10)
        elif 'qq' in flag:
            print("QQ:952447178")
            time.sleep(10)
        else:
            print("您的输入有误哦~~悄悄告诉你,可以从输入里获取彩蛋哦~")
            time.sleep(10)

if __name__=='__main__':
    print("------------------Welcome----------------")
    print('当前版本:',__version__,'\n作者:',__author__,'\n')
    print(__change__)
    time.sleep(3)
    print('······现在是登录模块·······')
    loginPro()
    threading.Thread(target=checkFin).start()
    option()  # 菜单信息
