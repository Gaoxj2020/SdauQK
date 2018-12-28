import requests
from lxml import etree
import time
from PIL import Image
import csv
import _thread
import sys
import os

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

def getyzm(session):
    try:
        imgres = session.get('http://jw.sdau.edu.cn/validateCodeAction.do?random=0.32352508289837867',timeout = 10)
        with open('yzm.jpg', 'wb') as imgfile:
            imgfile.write(imgres.content)
        time.sleep(4)
        img = Image.open('yzm.jpg')
        img.show()
        return session
    except:
        print('error:请接通学校内网..')
        exit(0)


def loginPro(session):
    userid = input('请输入学号\n')
    password = input('请输入密码\n')
    yzm = input('请输入验证码:\n')
    print('正在登陆..请稍后\n')
    session = login(session, userid, password, yzm)
    return session


def getAvaClass(session):
    classList = []
    session.get(url = xuckurl)
    for i in range(1,13):
        res = session.get(url=xkurl+str(i))
        html = etree.HTML(res.text)
        classhtml = html.xpath("//form[@name='xkActionForm']//table[@class='displayTag']//tr[contains(@class,'odd') or contains(@class,'even')]")
        for i,item in enumerate(classhtml):
            list =  item.xpath(".//td")
            classes = {}
            if len(list)>8:
                classes['xf'] = ''.join(list[5].text.split())
                classes['kch'] = ''.join(list[2].text.split())
                if not list[3].xpath(".//span") == []:
                    classes['kcm'] = list[3].xpath(".//span")[0].text.replace('\n', '').replace('\t', '').replace('\r', '')
                else:
                    classes['kcm'] = list[3].xpath('.//a')[0].text.replace('\n', '').replace('\t', '').replace('\r', '')
                classes['teacher'] = list[8].text.replace('\s', '').replace('\n', '').replace('\t', '').replace('\r', '')
                classList.append(classes)
    return classList

def saveAsFile(classList,i):
    if i==1:
        name = '可选课程'
    else:
        name = '已选课程'
    with open(name+'.csv','w+',encoding='utf-8-sig') as file:
        writer = csv.writer(file)
        writer.writerow(['课程号','课程名','课学分','教师'])
        for item in classList:
            writer.writerow([item['kch'],item['kcm'],item['xf'],item['teacher']])
    print('--已经导出到:',sys.path[0],'\\',name,'.csv\n')
def chooseClass(session,kcid):
    datas = {
        'kcId': kcid,
        'preActionType': '3',
        'actionType': '9',
    }
    res = session.post(url=chooseurl,data=datas)
    return res

def qk(session,kcid,pageNum,no):
    print('第',no,'个课程的抢课线程已开启.\n')
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
        try:
            res = chooseClass(session, kcid)
        except:
            print('error:抢课异常..')
            exit(0)
        time.sleep(3)

def getselectedClasses(session):
    res = session.get(url=selectedClasses)
    html = etree.HTML(res.text)
    list = html.xpath("//tr[@class = 'odd']")
    selectedClassesList = []
    for item in list:
        clas = {}
        l = item.xpath(".//td")
        if len(l) > 10:
            kch = l[1].text
            clas['kch'] = ''.join(kch.split())
            kcm = l[2].text
            clas['kcm'] = ''.join(kcm.split())
            xf = l[4].text
            clas['xf'] = ''.join(xf.split())
            teacher = l[7].text
            clas['teacher'] = ''.join(teacher.split())
            selectedClassesList.append(clas)
    return selectedClassesList

def login(session,userid,password,yzm):
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
        return session
    except:
        print('error:登录失败,原因未知')
        exit(0)

def checkFin(session,kcidList):
    List = getselectedClasses(session)
    time.sleep(10)
    flag1 = 0
    for item in List:
        for id in kcidList:
            if item['kch'] == id['kcid'][0:-2]:
                flag1 = flag1+1
                print("已经抢到",flag1,'门')
        if flag1<len(kcidList):
            return True
        elif flag1 >= len(kcidList):
            print('已经抢完所有课')
            return False
def inputInf(session):
    kcidList = []
    while (1):
        kc = {}
        kc['kcid'] = input('-----请输入所选择的课程号--格式: 课程号+下划线+课序号 如:XT108001_05: ')
        kc['pageNum'] = input('------请输入课程所在的页号: ')
        kcidList.append(kc)
        print('已输入',len(kcidList),'门课程。')
        flagE = input('继续输入吗？y/n')
        if flagE == 'n':
            break
    for i,item in enumerate(kcidList):
        _thread.start_new_thread(qk,(session,item['kcid'],item['pageNum'],i+1))
        time.sleep(2)
    return kcidList

def option(session,kcidList):
    flag1 = 1
    while flag1:
        flag = input("---输入1：退出系统·····\n"
                     "---输入2：查看导出所有可选课程信息到本地····\n"
                     "---输入3：查看并导出已选取所有课程信息·····\n"
                     "----------若不输入程序将保持抢课状态，待抢课完成自动退出--------\n")
        if flag == '1':
            exit(0)
        elif flag == '2':
            saveAsFile(getAvaClass(session),i=1)
        elif flag  == '3':
            saveAsFile(getselectedClasses(session),i=0)
        if flag1 ==0:
            print("抢课完成了！3S后退出..")
            time.sleep(3)
            sys.exit(0)

def main(session):

    print('··················现在开始抢课·····················')
    time.sleep(1)
    kcidList = inputInf(session)
    print("··········抢课已在新线程的后台开启，正在抢",len(kcidList),"门课···········\n")
    flag1 = True
    _thread.start_new_thread(option,(session,kcidList))# 菜单信息
    while flag1==True or flag1 ==None:
        flag1 = checkFin(session,kcidList)

    print('选课完成!系统5秒后退出..')
    time.sleep(5)
    print('---------------EXIT---------------')

if __name__=='__main__':
    session = requests.session()
    print('程序运行..正在加载验证码')
    session = getyzm(session)
    print('----验证码加载完毕，请查看后登录----')
    print('·······现在是登录模块·········')
    session = loginPro(session)
    main(session)
