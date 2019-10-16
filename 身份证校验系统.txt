from tkinter import *
import time,datetime

class IDCCheck():
    def __init__(self):
        self.win=Tk()
        self.win.geometry('700x400')
        self.win.title('身份验证系统')
        self.win['bg'] = 'lightblue'

        self.image = PhotoImage(file='11.png')
        self.lable_image=Label(self.win,image=self.image)
        self.lable_image.place(x=10,y=10)

        self.id_card=Label(self.win,text='请输入身份证号码：',font=('微软雅黑',14,'bold'),bg='navy',fg='lightblue')
        self.id_card.place(x=280,y=10,width=200)

        self.result_exits = StringVar()

        self.entry=Entry(self.win,textvariable=self.result_exits)
        self.entry.place(x=280,y=50,width=270,height=30)

        self.result_exits.set("")
        self.button_exits=Button(self.win,text='校验',font=('微软雅黑',12,'bold'),command=self.Id_birth,fg='navy')
        self.button_exits.place(x=580,y=45,width=60)

        self.result_true = StringVar()
        self.lable_check = Label(self.win, text='是否有效：', font=('微软雅黑', 14, 'bold'), fg="navy", bg="lightblue")
        self.lable_check.place(x=280, y=110)

        self.result_true.set("")
        self.entry_text = Entry(self.win,state=DISABLED,textvariable=self.result_true)
        self.entry_text.place(x=380, y=115,height=25,width=90)

        self.result_sex = StringVar()
        self.lable_sex = Label(self.win, text='      性别：', font=('微软雅黑', 14, 'bold'), fg="navy", bg="lightblue")
        self.lable_sex.place(x=280, y=160)

        self.result_sex.set("")
        self.entry_text1= Entry(self.win, state=DISABLED,textvariable=self.result_sex)
        self.entry_text1.place(x=380, y=165, height=25, width=90)

        self.result_birth = StringVar()
        self.lable_birth= Label(self.win, text='出生日期：', font=('微软雅黑', 14, 'bold'), fg="navy", bg="lightblue")
        self.lable_birth.place(x=280, y=210)
        self.result_birth.set("")

        self.entry_text2= Entry(self.win, state=DISABLED,textvariable=self.result_birth)
        self.entry_text2.place(x=380, y=215, height=25, width=210)

        self.reselt_address = StringVar()
        self.lable_address = Label(self.win, text='   所在地：', font=('微软雅黑', 14, 'bold'), fg="navy", bg="lightblue")
        self.lable_address.place(x=280, y=260)

        self.reselt_address.set("")
        self.entry_text3 = Entry(self.win, state=DISABLED,textvariable=self.reselt_address)
        self.entry_text3.place(x=380, y=275, height=25, width=210)

        self.button_quit = Button(self.win, text='关闭', command=self.Id_quit,font=('微软雅黑', 12, 'bold'), fg='navy')
        self.button_quit.place(x=530, y=330, width=60,height=30)

    def show(self):
        self.win.mainloop()

    def get_info(self):
        pass

    def Id_quit(self):
        self.win.quit()

    def Id_birth(self):
        try:
            list = ['0', '1', '2', '3', '4', '5', '6', '7', '8', '9', 'X']
            si_list = [7, 9, 10, 5, 8, 4, 2, 1, 6, 3, 7, 9, 10, 5, 8, 4, 2]
            si_list1 = ['1', '0', 'X', '9', '8', '7', '6', '5', '4', '3', '2']
            dt = {}
            with open('id_sql.txt', 'r') as dict_file:
                for line in dict_file:
                    (key, value) = line.strip().split(' ')
                    dt[key] = value

            a = self.result_exits.get()

            year = a[6:10]
            month = a[10:12]
            day = a[12:14]
            sex = a[16]
            address = a[0:6]
            number = a[0:17]
            of_number = 0
            for i in a:
                if i in list and len(a) == 18:

                    try:
                        if address in dt:
                            for x in dt:
                                if x == address:
                                    self.reselt_address.set(dt[x])
                                    break
                                continue
                        else:
                            year = 0
                        b = time.mktime(datetime.datetime(int(year), int(month), int(day)).timetuple())
                        y = time.mktime(datetime.datetime(1970, 1, 1, 8, 00).timetuple())
                        x = time.time()

                        if b >= y and b <= x:
                            self.result_true.set('有效')
                            self.result_birth.set("%s-%s-%s"%(year,month,day))
                            if int(sex) == 0 and int(sex) % 2 == 0:
                                self.result_sex.set("女")
                            else:
                                self.result_sex.set("男")

                        else:
                            self.result_true.set('无效')
                    except:
                        self.enabled()

                else:
                    self.enabled()

            for i in range(len(number)):
                of_number += int(number[i]) * int(si_list[i])
            of_number = of_number % 11
            if a[17:] != si_list1[of_number]:
                self.enabled()

        except:
            self.enabled()
    def enabled(self):
        self.result_true.set('无效')
        self.result_sex.set("")
        self.result_birth.set("")
        self.reselt_address.set("")

if __name__ == '__main__':
    long = IDCCheck()
    long.show()
