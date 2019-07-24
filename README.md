class Hero(object):
    def __init__(self, name, high, width, speed):
        self.name = name
        self.high = high
        self.width = width
        self.speed = speed

    def display_hero(self):
        print(self.name, " ", self.high, " ", self.width, " ", "初始速度：", self.speed)

    def action(self, fly, change_size):
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
    def action(self):
        shoot = True
        if shoot:
            self.blood -= 10
        else:
            self.blood = self.blood
        return self.blood
    def after_new_hero(self):
        print("正心4号血量变为：", self.blood)
new_hero = NewHero("正心4号", 21, 21, 200, 1000, 100)
new_hero.display_new_hero()
new_hero.action()
new_hero.after_new_hero()

class AnotherNewHero(NewHero):
    def __init__(self, name, high, width, speed, blood, energy):
        super().__init__(name, high, width, speed, blood, energy)
        # self.blood = blood
        # self.energy = energy

    def action(self, shoot):
        # shoot = True
        if shoot == 1:
            self.blood -= 10
            self.energy += 10
            return self.blood, self.energy
        else:
            self.blood = self.blood
            self.energy = self.energy
            return self.blood, self.energy
    def after_another_new_hero(self):
        print("正心5号血量变为：", self.blood, " ", "能量变为：", self.energy)
another_new_hero = AnotherNewHero("正心5号", 22, 22, 200, 1000, 0)
another_new_hero.display_new_hero()
another_new_hero.action(shoot = 1)
another_new_hero.after_another_new_hero()
