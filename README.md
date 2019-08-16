***列表中数据的最大值和平均值***
def avg_max(list):
    total = 0
    max = list[0]
    for i in list:
        total += i
        if max < i:
            max = i
    return total / len(list), max
print(avg_max([1, 2, 3, 4]))

***求二维数组里面每一行的平均值和最大值***
def avg_max1(list1):
    result = []
    for i in list1:
        r = avg_max(i)
        result.append(r)
    return result
print(avg_max1([[1, 2, 3, 4],
           [5, 2, 9, 4],
           [1, 4, 3, 4],
           [1, 2, 3, 7]]))

---------------------------------------------------------------------------------------------------------

***TCP***
import socket
# 创建一个socket对象
tcp_client_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
# 链接服务器
tcp_client_socket.connect(('localhost', 8080))
# 发送数据
tcp_client_socket.send('hello world !'.encode('gbk'))
# 接受数据
data = tcp_client_socket.recvfrom(1024)
print(data[0].decode('gbk'))
# 关闭数据
tcp_client_socket.close()

---------------------------------------------------------

from socket import *
# 创建socket
tcp_client_socket = socket(AF_INET, SOCK_STREAM)
# 目的信息
server_ip = input("请输入服务器ip：")
server_port = int(input("请输入服务器port："))
# 链接服务器
tcp_client_socket.connect((server_ip, server_port))
# 提示用户输入数据
send_data = input("请输入要发送的数据：")
tcp_client_socket.send(send_data.encode('gbk'))
# 接受对方发送过来的数据，最大接受1024个字节
recvData = tcp_client_socket.recv(1024)
print("接受到的数据为：", recvData.decode('gbk'))
# 关闭套接字
tcp_client_socket.close()

----------------------------------------------------------------------------------------------

***爬豆瓣Top250  保存 txt、csv、json***
import requests
import re
import csv
import json


class DouBan(object):

    def get_html(self,base_url):
        response = requests.get(base_url)
        return response

    def parsel_html(self, html):
        dds = re.findall(
            '<img width="100" alt="(.*?)" src=".*?<p class=""> (.*?)...<br>.*?"v:average">(.*?)</span>.*?</a>',
            html, re.S)
        print(dds)
        for d in dds:
            yield d[0] + d[1] + d[2]

    def write(self, dds, file_name):
        with open(file=file_name, mode='a', encoding='utf-8') as f:
            f.write(str(dds))
            f.write('\n')

    def write_csv(self):
        with open("豆瓣.csv", 'w+', encoding='utf-8') as csvfile:
            spamwriter = csv.writer(csvfile, dialect='excel')
            with open('豆瓣.txt', 'r', encoding='utf-8') as file:
                for line in file:
                    line_list = line.strip('\n').split(',')
                    spamwriter.writerow(line_list)

    def write_json(self):
        csvfile = open('豆瓣.csv', 'r', encoding='utf-8')
        jsonfile = open('豆瓣.json', 'w', encoding='utf-8')
        fieldnames = ('电影名称', '导演', '主演和评分')
        reader = csv.DictReader(csvfile, fieldnames)
        for row in reader:
            json.dump(row, jsonfile, ensure_ascii=False)
            jsonfile.write('\r\n')

    def main(self):
        for i in range(0, 250, 25):
            response = self.get_html(base_url='https://movie.douban.com/top250?start=%s&filter=' % i)
            # print(response)
            dds = self.parsel_html(response.text)
            for i in dds:
                self.write(i, "豆瓣.txt")
        dou.write_csv()
        dou.write_json()


if __name__ == '__main__':
    dou = DouBan()
    dou.main()
    
    -------------------------------------------------------------------------------------------------
    
***多线程***
import time
import threading
import requests
import re
import csv
import json


class DouBan(object):

    def get_html(self,base_url):
        response = requests.get(base_url)
        return response

    def parsel_html(self, html):
        dds = re.findall(
            '<img width="100" alt="(.*?)" src=".*?<p class=""> (.*?)...<br>.*?"v:average">(.*?)</span>.*?</a>',
            html, re.S)
        # print(dds)
        for d in dds:
            yield d[0] + d[1] + d[2]

    def write(self, dds, file_name):
        with open(file=file_name, mode='a', encoding='utf-8') as f:
            f.write(str(dds))
            f.write('\n')

    def write_csv(self):
        with open("豆瓣.csv", 'w+', encoding='utf-8') as csvfile:
            spamwriter = csv.writer(csvfile, dialect='excel')
            with open('豆瓣.txt', 'r', encoding='utf-8') as file:
                for line in file:
                    line_list = line.strip('\n').split(',')
                    spamwriter.writerow(line_list)

    def write_json(self):
        csvfile = open('豆瓣.csv', 'r', encoding='utf-8')
        jsonfile = open('豆瓣.json', 'w', encoding='utf-8')
        fieldnames = ('电影名称', '导演', '主演和评分')
        reader = csv.DictReader(csvfile, fieldnames)
        for row in reader:
            json.dump(row, jsonfile, ensure_ascii=False)
            jsonfile.write('\r\n')

    def main(self, i):
        # for i in range(0, 250, 25):
        # response = dou.get_html(base_url='https://movie.douban.com/top250?start=%s&filter=' % i)
        # dds = dou.parsel_html(response.text)
        for i in dds:
            dou.write(i, "豆瓣.txt")
        dou.write_csv()
        dou.write_json()


if __name__ == '__main__':
    dou = DouBan()
    start_time = time.time()
    for i in range(0, 250, 25):
        response = dou.get_html(base_url='https://movie.douban.com/top250?start=%s&filter=' % i)
        dds = dou.parsel_html(response.text)
        dou.main(o)
    print('单线程运行时间', time.time() - start_time)
    start_time = time.time()
    # base_url = 'https://movie.douban.com/top250?start={}&filter='
    for i in range(0, 250, 25):
        base_url = 'https://movie.douban.com/top250?start=%s&filter=' % i
        thread = threading.Thread(target=dou.main(o), args=(base_url.format(i),))
        thread.start()
    print('多线程运行时间', time.time() - start_time)   （应该是有问题的）
    
    -----------------------------------------------------------------------------------------------------
    
from cmd import Cmd


class Example(Cmd):
    # prompt = ''
    intro = '看运气的时候到啦，输入range[0,10],看看是不是你想要的结果'

    # list_1 将`['1', '2', '3', '4']`变为数字数组
    # list_2 将`'5678'`变为数字数组
    def __init__(self):
        super().__init__()
        self.list_1 = list(map(int, ['1', '2', '3', '4']))
        self.list_2 = list(map(int, (x for x in '5678')))

    # list_map 分别返回两个列表（列表里面都是数字）
    def do_0(self, arg):
        print("大表哥:{}".format(self.list_1), "大表姐：{}".format(self.list_2))

    # list_map_sort 分别逆序返回两个列表（列表里面都是数字）
    def do_1(self, arg):
        self.list_1.sort(reverse=True)
        self.list_2.sort(reverse=True)
        print("倒立的大表哥和大表姐", self.list_1, self.list_2)

    # list_max 分别返回两个列表中的最大值
    def do_2(self, arg):
        a = max(self.list_1)
        b = max(self.list_2)
        print("大表哥和大表姐最幸运的数字：{} 和 {}".format(a, b))

    # list_sum 分别返回两个列表的和
    def do_3(self, arg):
        c = sum(self.list_1)
        d = sum(self.list_2)
        print("大表哥和大表哥：{}，大表姐和大表姐：{}".format(c, d))

    def do_4(self, arg):
        print("哈哈哈哈哈哈哈")

    # list_filter 分别返回两个列表中的偶数位
    def do_5(self, arg):
        list_1 = list(filter(lambda i: i % 2 == 0, self.list_1))
        list_2 = list(filter(lambda i: i % 2 == 0, self.list_2))
        print("大表哥和大表姐说偶数：", list_1, list_2)

    # list_zip 将两个列表合并后返回
    def do_6(self, arg):
        list_3 = list(zip(self.list_1, self.list_2))
        print("大表哥和大表姐在一起了：{}".format(list_3))

    # list_to_dict 将两个列表变成字典返回，list_1作为健list_2作为值
    def do_7(self, arg):
        dictionary = dict(zip(self.list_1, self.list_2))
        print("神秘的大表哥和大表姐", dictionary)

    def do_8(self, arg):
        print("不明白cmd为什么需要arg（隐形的箭头指向上一行的arg）")
        print("老师明亮的双目一定要看到上一行，求解求解")

    def do_9(self,arg):
        d = D()
        print(d.list_1_set)
        i = input("改造大表哥（每输入一个数字跟上一个空格）：").split(' ')
        d.list_1_set = list(map(int, (x for x in i)))
        # d.list_1_set = [9, 10, 11, 12]
        print("改造后的大表哥：{}".format(d.list_1_set))

    def do_10(self, arg):
        print("敬个礼吖，握握手，你是我的好朋友")
        return 1

    def start(self):
        self.cmdloop()


# list_1_set 将此类方法设置为属性，访问是用过`对象.list_1_set = [0, 0, 0, 0] ` 就可以修改 `list_1` 的值
class D(Example):

    def __init__(self):
        super().__init__()
        self._list_1_set = [0, 0, 0, 0]

    @property
    def list_1_set(self):
        return self._list_1_set

    @list_1_set.setter
    def list_1_set(self, list):
        self._list_1_set = list

    @list_1_set.deleter
    def list_1_set(self):
        del self._list_1_set


if __name__ == '__main__':
    example = Example()
    example.start()
    # d = D()

----------------------------------------------------------------------------------------

"""
写出一个用户名敏感词过滤装饰器，需要过滤敏感词见 `敏感词表(广告).txt`
要求使用类装饰器实现，在实例化对象是，一旦名字中出现敏感词表中的内容，提示不能创建对象（或抛出异常）
"""


class Name:
    def __init__(self, name):
        self.name = name

    def __str__(self):
        return self.name


class Timer:
    def __init__(self, func):
        self.func = func

    def __call__(self, *args, **kwargs):
        with open('敏感词表(广告).txt', mode='r', encoding='utf-8') as f:
            data = f.read().split('\n')
            # print(data)
        for i in data:
            if i not in kwargs['name'].name:
                return self.func(*args, **kwargs)
            else:
                print('不允许使用该用户名，请重试')
                name = Name(str(input("您可以使用字母、数字和英文句点：")))
                success(name=name)
                break

@Timer
def success(name=None):
    print("%s，创建成功" % name)



name = Name(str(input("您可以使用字母、数字和英文句点：")))
success(name=name)

    
    
    
    
    
    
    
    
    

