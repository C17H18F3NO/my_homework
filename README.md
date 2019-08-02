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

***类的练习***
class Hero(object):
    def __init__(self, name, high, width, speed):
        """初始化方法  初始化类属性"""
        self.name = name
        self.high = high
        self.width = width
        self.speed = speed

    def display_hero(self):
        print(self.name, " ", self.high, " ", self.width, " ", "初始速度：", self.speed)

    def action(self, fly, change_size):
        """行为  飞行   改变大小"""
        # fly = True
        # change_size = True
        if fly == 1 and change_size == 0:
            self.speed += 2
            return self.speed
        elif fly == 0 and change_size == 1:
            self.high += 1
            self.width += 1
            return self.high, self.width
        else:
            print("给您一个蒙娜丽莎的微笑，提醒您输入有误")
    def after_hero(self):
        print(self.name, " ", self.high, " ", self.width, " ", "此时速度：", self.speed)

hero_one = Hero("正心1号",  15, 20, 200)
hero_two = Hero("正心2号", 20, 20, 200)
hero_three = Hero("正心3号", 19, 19, 200)
hero_one.display_hero()
hero_two.display_hero()
hero_three.display_hero()
hero_one.action(fly = 1, change_size = 1)
hero_one.after_hero()

class NewHero(Hero):
    def __init__(self, name, high, width, speed, blood, energy):
        Hero.__init__(self, name, high, width, speed)
        self.blood = blood
        self.energy = energy
    def display_new_hero(self):
        print(self.name, " ", self.high, " ", self.width, " ",
              "初始速度：", self.speed, " ", self.blood, " ", self.energy)
    # def action(self, shoot):  重写父类的action 但是会有警告 因为跟父类的参数个数不同 所以下面action1为重写的函数
    def action1(self, shoot):
        # shoot = True
        if shoot == 1:
            self.blood -= 10
            # return self.blood
            if self.blood <= 0:
                self.blood = 0
                print("英雄重回泉眼再生")
            return self.blood
        else:
            self.blood -= 10
            self.energy += 10
            if self.blood <= 0:
                self.blood = 0
                print("英雄重回泉眼再生")
            if self.energy >= 100:
                self.energy = 100
            return self.blood, self.energy
            # self.blood = self.blood
        # return self.blood
    def after_new_hero(self):
        print("正心4号血量变为：", self.blood)
new_hero = NewHero("正心4号", 21, 21, 200, 1000, 100)
new_hero.display_new_hero()
new_hero.action1(shoot = 1)
new_hero.after_new_hero()

class AnotherNewHero(NewHero):
    def __init__(self, name, high, width, speed, blood, energy):
        super().__init__(name, high, width, speed, blood, energy)
        # self.blood = blood
        # self.energy = energy

    # def action(self, shoot):
    #     # shoot = True
    #     if shoot == 1:
    #         self.blood -= 10
    #         self.energy += 10
    #         return self.blood, self.energy
    #     else:
    #         self.blood = self.blood
    #         self.energy = self.energy
    #         return self.blood, self.energy
    def after_another_new_hero(self):
        print("正心5号血量变为：", self.blood, " ", "能量变为：", self.energy)
another_new_hero = AnotherNewHero("正心5号", 22, 22, 200, 1000, 0)
another_new_hero.display_new_hero()
another_new_hero.action1(shoot = 0)
another_new_hero.after_another_new_hero()


-------------------------------------------------------------------------------------------------

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
            '<span class="title">(.*?)</span>.*?导演:(.*?)主演:(.*?)<br>.*?<div class="star">.*?average">(.*?)</span>.*?</div>',
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

