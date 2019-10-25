#目标：将知乎收藏夹里的内容下载下来（垃圾知乎看个收藏夹都那么费劲）

#输入：收藏夹链接
#输出：多个文本

#思路：
#1.得到一个url，把所有答案链接放到一个列表 OK 
#2.用一个函数，输入答案链接，输答案的文本内容 OK
#2.5 把答案的文本内容输出到文件
#3.修改2，输出包含图片和链接的其他文本
import requests
import re
import os
from bs4 import BeautifulSoup
def getHTMLText(url):
    try:
        kv={'user-agent':'Mozilla/5.0'}#标识游览器的种类，Mozilla/5.0是标准游览器字段
        print("正在获取\n")
        r=requests.get(url,timeout=30,headers = kv)#使用get方法获取字符串
        r.raise_for_status()#如果状态码不为200，引发异常
        # r.encoding=r.apparent_encoding
        print("获取结束\n")
        return r.text#返回前1000的字符
    except:
        return "产生异常"

def NO1(url):
    print("请输入页数")
    n=input()
    n= int(n)
    lin=[]
    for i in range (n):
        s=str(i+1)
        url_now=url+"?page="+s
        print(url_now)
        demo=getHTMLText(url_now)
        soup=BeautifulSoup(demo,"html.parser")
        sl=soup.find_all('link',itemprop=re.compile('url'))#读取所有名字为link，字典 itemprop=“url”的元素做成一个列表
        for a in sl:
            b=str(a).split('"')
            lin.append(b[1])
        #print(lin)
        #print(sl)#这里的sl是一个包含答案链接的列表
    return lin



def NO2(url,text_t):#这里是输入一个答案的url，输出带问题名字的答案的文本（返回一个str）
    r=getHTMLText(url)
    bs=BeautifulSoup(r,"html.parser")
    title=bs.title                         # 获取标题
    filename_old=title.string.strip()
    filename=re.sub('[\/:*?"<>|]', '-', filename_old)      # 用来保存内容的文件名，因为文件名不能有一些特殊符号，所以使用正则表达式过滤掉
    text_t=text_t+filename+"\n\n"
    answer=bs.find("div",class_="RichContent-inner")
    for each_answer in answer:
	    for a in each_answer.strings:#从内容中提取答案并输出到text中来
		    text_t=text_t+"\n"+a
    text_t=text_t+"\n\n*************************************\n\n"
    return text_t






if __name__ == "__main__" :#当你是通过import的方法来执行这个.py文件时，不能运行if __name__ == '__main__':下面的语句或者函数.
    url="https://www.zhihu.com/collection/362262332"
    sl=NO1(url)#注意这里的sl的url不能直接用
    text1="\n************************\n"#这里的文本开头
    for url2 in sl:
        url1="https://www.zhihu.com"+url2
        text1=NO2(url1,text1)
    print(text1)
