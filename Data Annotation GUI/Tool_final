#!/usr/bin/env python3
# -*- coding: utf-8 -*-
"""
Created on Wed Jun 30 23:25:44 2021

@author: apple
"""

import pandas as pd 
import os
from os import path
import tkinter as tk
from tkinter import filedialog
import tkinter.messagebox as messagebox



class simpleapp_tk(tk.Tk):
    def __init__(self,parent):
        tk.Tk.__init__(self,parent)
        self.parent = parent
        self.index = 0
        self.flag = 0

        self.datafile = datafile #name of the file currently working on
        self.result_df = d #dataframe

        # self.speaker = self.result_df["Speaker"]
        self.text = self.result_df["text"]
        self.record = self.result_df["Record"]


        #intialize item attribute
        self.item = tk.StringVar()

        self.progress = tk.StringVar()
        #self.variables = []
        self.vaccinename= tk.StringVar()
        self.advertisement= tk.StringVar()
        self.comment = tk.StringVar()
        self.willingness = tk.StringVar()
        self.attitude = tk.StringVar()

        self.multi_vacc = {}
        self.labeled_vacc = tk.StringVar()
        self.labeled_vacc_string = ''

        #defining side effects choices and vars
        self.sideeffects_choices= ["Headache", "Redness","Swelling", "Tiredness", 
        "Muscle Pain", "Chills","Fever","Nausea", "None","No Details", "Other"]


        #creating variables
        self.headache_var = tk.StringVar(value = "off")
        self.redness_var = tk.StringVar(value = "off")
        self.swelling_var = tk.StringVar(value = "off")
        self.tiredness_var = tk.StringVar(value = "off")
        self.muscle_pain_var = tk.StringVar(value = "off")
        self.chills_var = tk.StringVar(value = "off")
        self.fever_var = tk.StringVar(value = "off")
        self.nausea_var = tk.StringVar(value = "off")
        self.none_var = tk.StringVar(value = "off")
        self.no_details_var = tk.StringVar(value = "off")
        self.other_var = tk.StringVar(value = "off")
        
        self.sideeffects_vars = [self.headache_var, self.redness_var, self.swelling_var, self.tiredness_var, 
                            self.muscle_pain_var, self.chills_var, self.fever_var, self.nausea_var, 
                            self.none_var, self.no_details_var, self.other_var]
        


#Frame:textbox
        textFrame = tk.Frame(self)
        textFrame.grid(column=0, row = 0, rowspan = 25)
        #show the titile of the datafile
        label = tk.Label(textFrame, text = "Current file: "+ self.datafile)
        label.grid(column = 0, row = 0, sticky = "sw")
        #setup the textbox        
        self.textbox = tk.Text(textFrame, height=35, bg = "#f1f8e9", wrap = tk.WORD)
        #3 styles of the text display
        self.textbox.tag_configure("num", font=("Arial", 20, "bold"))
        self.textbox.tag_configure("normal", font = ("Arial", 24))
        self.textbox.grid(column=0, row=1, rowspan = 25)

        #initiate the progress bar
        self.progress = tk.StringVar()

        #display the textbox
        self.DisplayData()

        #display the annotation labels
        self.initialize()

        #when press x to close window, save before close
        self.protocol("WM_DELETE_WINDOW", self.on_closing)

    
    def DisplayData(self):

        self.textbox.configure(state='normal')
        self.textbox.delete(0.0,'end')
        
        self.prior = max(0, self.index-0)
        self.post = min(len(self.record), self.index+1)

        for i in range(self.prior, self.post):
            self.textbox.insert("end","Index #" + str(self.index + 1) + " -- Post #" + str(self.record[i] + 1) + ": \n\n", "num")
            self.textbox.insert("end", self.text[i]+"\n", "normal")
        
        self.textbox.configure(state='disabled')
        
        

    #if there's existing data, read in and set value
    def ShowExisting(self):

        self.advertisement.set(self.result_df["Advertisement"][self.index])

        self.labeled_vacc_string = ''

   # check existing side effects
            # if self.result_df["Side Effects"][self.index] != "":
            #     results = self.result_df["Side Effects"][self.index].split(",")


        if self.result_df["Advertisement"][self.index] == "Yes":
            self.WillingnessStatus("disabled")
            self.AttitudeStatus("disabled")
            self.VaccineNameStatus("disabled")
            self.next_vaccine_buttonStatus("disabled")
            self.check_next_vaccine_button.configure(state="disabled")

            
            self.SideEffectsStatus("disabled")
            for i in range(len(self.sideeffects_vars)):
                self.sideeffects_vars[i].set(value = "off")

        else:
            self.WillingnessStatus("normal")
            self.AttitudeStatus("normal")
            self.VaccineNameStatus("normal")
            self.SideEffectsStatus("normal")
            self.next_vaccine_buttonStatus("normal")
            self.check_next_vaccine_button.configure(state="normal")
            if isinstance(self.result_df["label"][self.index], str):
                self.result_df["label"][self.index] = eval(self.result_df["label"][self.index])

            try:
                self.vaccinename.set(list(self.result_df['label'][self.index].keys())[0])
                self.comment.set(list(self.result_df['label'][self.index].values())[0]['Comments'])
                self.attitude.set(list(self.result_df['label'][self.index].values())[0]['Attitude'])
                self.willingness.set(list(self.result_df['label'][self.index].values())[0]['Willingness'])


                if len(list(self.result_df['label'][self.index].keys())) > 0:
                    for i in self.result_df['label'][self.index].keys():
                        self.labeled_vacc_string += ' ' + i
                    self.labeled_vacc.set(self.labeled_vacc_string)

                if list(self.result_df['label'][self.index].values())[0]['Side Effects'] != "":
                    results = list(self.result_df['label'][self.index].values())[0]['Side Effects'].split(",")

                    for i in range(len(results)):
                        ind = self.sideeffects_choices.index(results[i])
                        self.sideeffects_vars[ind].set("on")

            except:
                self.vaccinename.set('')
                self.comment.set('')
                self.attitude.set('')
                self.willingness.set('')
                self.labeled_vacc.set('')

    #initialize the buttons

    def initialize(self):
        self.grid()
        
        #list of all buttons
        self.radios=[]
        
        
        #list of willingness and attitude buttons
        self.willingness_buttons =[]
        self.attitude_buttons =[]
        self.advertisement_buttons = []
        self.vaccineName_buttons = []
        self.sideeffects_buttons = []



        #buttons; ("label","writing in the coding file")
        advertisements = [("Yes","Yes"), ("No", "No")]
        vaccineNames = [("Pfizer","Pfizer"),("Moderna","Moderna"),("AstraZeneca","AstraZeneca"), ("Sinovac", "Sinovac")]

        #build the buttons
        #set up the row for the buttons
        i = 0

        #advertisement buttons
        #label the category
        self.advertisement_label = tk.Label(self,text="Is this an advertisement?", font = ("Arial", 15, "bold"))
        self.advertisement_label.grid(column=1,row=i,sticky="s",columnspan=2)
        i+=1
        #build buttons for advertisement
        for text,value in advertisements:
            b = tk.Radiobutton(self,text=text,variable=self.advertisement,
                value=value,indicatoron=0,width=10,height=1, 
                command = lambda:self.EnableAdvertisement())
            b.grid(column=1,row=i,columnspan=2)
            self.advertisement_buttons.append(b)
            self.radios.append(b)
            i+=1

        #setup the buttons for vaccine name
        self.vaccineName_label = tk.Label(self,text="Vaccine Name", font = ("Arial", 15, "bold"))
        self.vaccineName_label.grid(column=1,row=i,sticky="s",columnspan=2)
        i+=1
        for text,value in vaccineNames:
            b = tk.Radiobutton(self,text=text,variable=self.vaccinename,
                value=value,indicatoron=0,width=10,height=1)
                # command = lambda: self.EnableSubI())
            b.grid(column=1,row=i,columnspan=2)
            self.vaccineName_buttons.append(b)
            self.radios.append(b)
            i+=1

        self.next_vaccine_button = tk.Button(self, text="Next vaccine",
                                     command=self.next_vaccine)
        self.next_vaccine_button.grid(column=1, row=9, columnspan=2)
        self.next_vaccine_button.configure(state="normal")

        self.check_next_vaccine_button = tk.Button(self, text="Check annotated vaccines",
                                             command=self.check_next_vaccine)
        self.check_next_vaccine_button.grid(column=3, row=9)
        self.check_next_vaccine_button.configure(state="normal")

        #记录已经标记的疫苗
        self.labeled_vaccineNames = tk.Label(self, textvariable=self.labeled_vacc, font=("Arial", 14, "bold"))
        self.labeled_vaccineNames.grid(column=1, row=11, columnspan=4)

        self.SideEffects()

        self.InitSubs()

        # if self.advertisement.get() == "Yes":
        #     self.WillingnessStatus("disabled")
        #     self.AttitudeStatus("disabled")
        #     self.VaccineNameStatus("disabled")
        #     self.SideEffectsStatus("disabled")
        #     self.next_vaccine_buttonStatus("disabled")
        # else:
        #     self.WillingnessStatus("normal")
        #     self.AttitudeStatus("normal")
        #     self.VaccineNameStatus("normal")
        #     self.SideEffectsStatus("normal")
        #     self.next_vaccine_buttonStatus("normal")

#Frame: optional
        optFrame = tk.Frame(self)
        optFrame.grid(column = 4, row = i, columnspan = 4, rowspan =13)

        #comment button
        label = tk.Label(self,text="Comments (optional):", font = ("Arial", 15, "bold"))
        label.grid(column=5,row=14,columnspan=2)
        self.entry = tk.Entry(self,textvariable=self.comment, width = 30)
        self.entry.grid(column=5,row=15,sticky="n",columnspan=3, rowspan = 10)

#Frame: buttons
        #Frame placed at the bottom rightcorner
        bottomFrame = tk.Frame(self)
        bottomFrame.grid(column = 3, row = 24)
        #previous button
        #use "self.index" as the go to number; Goto function will subtract 1 anyway
        #save the currect selection
        self.button_prev = tk.Button(bottomFrame,text=u"Prev",
                command = lambda: self.Goto(self.index))
        self.button_prev.grid(column=1,row=1)
        self.button_prev.configure(state="normal")
        self.bind("<Left>", lambda i: self.Goto(self.index)) 

        #next button, record results to df and reinitialize
        self.button_next = tk.Button(bottomFrame,text=u"Next",
                command = lambda: self.Goto(self.index+2))       
        self.button_next.grid(column=2,row=1)
        self.button_next.configure(state="normal")
        self.bind("<Right>", lambda i: self.Goto(self.index+2)) 

        #progress bar        
        label = tk.Label(bottomFrame,textvariable=self.progress)   #windows下显示有问题
        label.grid(column=3,row=1,sticky="s")

        #goto button
        num_label = tk.Label(bottomFrame, text = "Go to Index #: ")
        num_label.grid(column=1,row=2,sticky="s")
        self.num = tk.StringVar()
        self.num.set("")
        self.entry_num = tk.Entry(bottomFrame,textvariable=self.num, width = 5)
        self.entry_num.grid(column=2,row=2,sticky="n", columnspan=1)
        self.goto_button = tk.Button(bottomFrame, text=u"Confirm", 
            command = lambda: self.Goto(self.num.get()))
        self.goto_button.grid(column=3,row=2)
        self.goto_button.configure(state="normal")

        #save button,save df to csv
        self.button_save = tk.Button(bottomFrame,text=u"Save",
                                command=lambda: self.Save())
        self.button_save.grid(column=1,row=3)
        self.button_save.configure(state="normal")

        #exit button
        self.button_exit = tk.Button(bottomFrame,text=u"Exit",
                                command=lambda: self.Quit())
        self.button_exit.grid(column=2,row=3)
        self.button_exit.configure(state="normal")

        #if the catgory has recorded annotation, display previous annotation
        self.ShowExisting()
        #set everything in place
        self.grid_columnconfigure(0,weight=1)
        self.resizable(True,True)
        self.update()

        
    #subcategory buttons
    def InitSubs(self):
        subFrame = tk.Frame(self)
        subFrame.grid(column = 3, row = 1, columnspan = 2, rowspan = 9,  sticky = "nw")
        
        willingness_answers = ["Yes", "No", "Not sure"]
        i = 0
        self.willingness_label = tk.Label(self, text = "Willingness", font = ("Arial", 15, "bold"))
        self.willingness_label.grid(column = 3, row = i, sticky = "s", columnspan = 1)
        i += 1
        for text in willingness_answers:
            b = tk.Radiobutton(self, text=text, variable =self.willingness, 
                value = text, indicatoron = 0, width=15,height=1)
            b.grid(column=3, row = i, columnspan=1)
            self.willingness_buttons.append(b)
            self.radios.append(b)
            i += 1
        
        attitudes = ["Positive", "Negative", "Neutral"]
        self.attitude_label = tk.Label(self, text = "Attitude", font = ("Arial", 15, "bold"))
        self.attitude_label.grid(column = 3, row = i, sticky = "s", columnspan = 1)
        i += 1
        for text in attitudes:
            b = tk.Radiobutton(self, text=text, variable =self.attitude, 
                value = text, indicatoron = 0, width=15,height=1)
            b.grid(column=3, row = i, columnspan=1)
            self.attitude_buttons.append(b)
            self.radios.append(b)
            i += 1   

    def WillingnessStatus(self, status):
        for b in self.willingness_buttons:
            b.configure(state=status)
        self.willingness_label.configure(state = status)
        
    def AttitudeStatus(self,status):
        for b in self.attitude_buttons:
            b.configure(state=status)        
        self.attitude_label.configure(state = status)
        
    #advertisement status
    def AdvertisementStatus(self,status):
        for b in self.advertisement_buttons:
            b.configure(state=status)        
        self.advertisement_label.configure(state = status)
        
    #Vaccine Name status
    def VaccineNameStatus(self,status):
        for b in self.vaccineName_buttons:
            b.configure(state=status)        
        self.vaccineName_label.configure(state = status)
    
    #Side effects status
    def SideEffectsStatus(self,status):
        for b in self.sideeffects_buttons:
            b.configure(state=status)        
        self.sideeffects_label.configure(state = status)

    def next_vaccine_buttonStatus(self, status):
        self.next_vaccine_button.configure(state=status)
    

    #If advertisement, disable other labeling options
    def EnableAdvertisement(self):
        if self.advertisement.get() == "No":
            self.WillingnessStatus("normal")
            self.AttitudeStatus("normal")

            self.VaccineNameStatus("normal")
            self.SideEffectsStatus("normal")
            self.next_vaccine_buttonStatus("normal")
            self.check_next_vaccine_button.configure(state="normal")
        else:
            self.WillingnessStatus("disabled")
            self.willingness.set("")
            # self.result_df["Willingness"][self.index] = ""
            
            self.AttitudeStatus("disabled")
            self.attitude.set("")
            # self.result_df["Attitude"][self.index] = ""

            
            self.VaccineNameStatus("disabled")
            self.vaccinename.set("")
            # self.result_df["Vaccine Name"][self.index] = ""
            self.result_df["label"][self.index] = ""

            
            self.SideEffectsStatus("disabled")
            for i in range(len(self.sideeffects_vars)):
                self.sideeffects_vars[i].set(value = "off")
            # self.result_df["Side Effects"][self.index] = ""

            self.next_vaccine_buttonStatus("disabled")
            self.check_next_vaccine_button.configure(state="disabled")
            

    
    def SideEffects(self):        

        
        sideeffects_Frame = tk.Frame(self)
        sideeffects_Frame.grid(column = 5, row = 0, columnspan = 1, rowspan = 10,  sticky = "n")
        

        z = 0
        self.sideeffects_label = tk.Label(self, text = "Side Effects", font = ("Arial", 15, "bold"))
        self.sideeffects_label.grid(column = 5, row = z, sticky = "n", columnspan = 1)
        z += 1        

        
        for i in range(len(self.sideeffects_choices)):
            b = tk.Checkbutton(self, text= self.sideeffects_choices[i], variable = self.sideeffects_vars[i], 
                               onvalue = "on", offvalue = "off", width=15, height=1)
            b.grid(column=5, row = z, columnspan=1)
            self.sideeffects_buttons.append(b)
            self.radios.append(b)
            z += 1    
            

    #record the button click to results_df
    def dfResults(self):

        adv = self.advertisement.get()
        vaccine = self.vaccinename.get()
        willingness = self.willingness.get()
        attitude = self.attitude.get()
        comment = self.comment.get()
        if adv != 'No':
            self.result_df["Advertisement"][self.index] = self.advertisement.get()
            self.result_df["label"][self.index] = {}
            return 1
        else:
            temp = []
            if vaccine == '':
                messagebox.showwarning("警告", "请标注疫苗类型")
                return 0
            else:
                self.labeled_vacc.set('')
                for i in range(len(self.sideeffects_vars)):
                    if self.sideeffects_vars[i].get() == "on":
                        temp.append(self.sideeffects_choices[i])

                try:
                    self.multi_vacc[vaccine]['Side Effects'] = ','.join(temp)
                    self.multi_vacc[vaccine]['Willingness'] = willingness
                    self.multi_vacc[vaccine]['Attitude'] = attitude
                    self.multi_vacc[vaccine]['Comments'] = comment
                except:
                    self.multi_vacc[vaccine] = {}
                    self.multi_vacc[vaccine]['Side Effects'] = ','.join(temp)
                    self.multi_vacc[vaccine]['Willingness'] = willingness
                    self.multi_vacc[vaccine]['Attitude'] = attitude
                    self.multi_vacc[vaccine]['Comments'] = comment

                #write in the data
                self.result_df["Advertisement"][self.index] = self.advertisement.get()
                if isinstance(self.result_df["label"][self.index],str):
                    self.result_df["label"][self.index] = eval(self.result_df["label"][self.index])

                self.result_df["label"][self.index][vaccine] = self.multi_vacc[vaccine]
                return 1


    #save currect selection, jump to the Record number given
    def Goto(self,num):
        #save
        flag = self.dfResults()
        if flag:
            self.flag = 0 #用于检查疫苗
            self.multi_vacc = {}
            #make sure the number doesn't exist 0 to the length of the dataset
            if int(num) < 1:
                self.index = 0
            elif int(num)>len(self.record):
                self.index = len(self.record)-1
            else:
                self.index = int(num)-1
            #setup next dataviewbox
            self.DisplayData()
            #reset buttons, columns
            self.comment.set("")

            for b in self.radios:
                b.deselect()
            #show existing
            self.ShowExisting()

    def next_vaccine(self):
        # adv = self.advertisement.get()
        vaccine = self.vaccinename.get()
        willingness = self.willingness.get()
        attitude =  self.attitude.get()
        comment = self.comment.get()

        temp = []

        if vaccine == '':
            messagebox.showwarning("警告", "请标注疫苗类型")
        else:
            if vaccine not in self.labeled_vacc_string:
                self.labeled_vacc_string += ' ' + vaccine
                self.labeled_vacc.set(self.labeled_vacc_string)
            for i in range(len(self.sideeffects_vars)):
                if self.sideeffects_vars[i].get() == "on":
                    temp.append(self.sideeffects_choices[i])

            try:
                self.multi_vacc[vaccine]['Side Effects'] = ','.join(temp)
                self.multi_vacc[vaccine]['Willingness'] = willingness
                self.multi_vacc[vaccine]['Attitude'] = attitude
                self.multi_vacc[vaccine]['Comments'] = comment
            except:
                self.multi_vacc[vaccine] = {}
                self.multi_vacc[vaccine]['Side Effects'] = ','.join(temp)
                self.multi_vacc[vaccine]['Willingness'] = willingness
                self.multi_vacc[vaccine]['Attitude'] = attitude
                self.multi_vacc[vaccine]['Comments'] = comment
            if isinstance(self.result_df["label"][self.index], str):
                self.result_df["label"][self.index] = eval(self.result_df["label"][self.index])

            self.result_df["label"][self.index][vaccine] = self.multi_vacc[vaccine]

        self.vaccinename.set('')
        self.advertisement.set('No')
        self.comment.set('')
        self.attitude.set('')
        self.willingness.set('')
        for i in self.sideeffects_vars:
            i.set('off')

    def check_next_vaccine(self):
        vaccine = self.vaccinename.get()
        willingness = self.willingness.get()
        attitude = self.attitude.get()
        comment = self.comment.get()

        temp = []
        if vaccine == '':
            messagebox.showwarning("警告", "请标注疫苗类型")

        else:
            for i in range(len(self.sideeffects_vars)):
                if self.sideeffects_vars[i].get() == "on":
                    temp.append(self.sideeffects_choices[i])

            try:
                self.multi_vacc[vaccine]['Side Effects'] = ','.join(temp)
                self.multi_vacc[vaccine]['Willingness'] = willingness
                self.multi_vacc[vaccine]['Attitude'] = attitude
                self.multi_vacc[vaccine]['Comments'] = comment
            except:
                self.multi_vacc[vaccine] = {}
                self.multi_vacc[vaccine]['Side Effects'] = ','.join(temp)
                self.multi_vacc[vaccine]['Willingness'] = willingness
                self.multi_vacc[vaccine]['Attitude'] = attitude
                self.multi_vacc[vaccine]['Comments'] = comment

            # write in the data
            self.result_df["Advertisement"][self.index] = self.advertisement.get()
            if isinstance(self.result_df["label"][self.index], str):
                self.result_df["label"][self.index] = eval(self.result_df["label"][self.index])

            self.result_df["label"][self.index][vaccine] = self.multi_vacc[vaccine]
            labeled_vac = ''
            for i in self.result_df["label"][self.index].keys():
                labeled_vac += ' ' + i
            self.labeled_vacc.set(labeled_vac)



        if len(self.result_df["label"][self.index].keys()) > 0:
            items = list(self.result_df["label"][self.index].items())
            len_ = len(items)
            self.flag += 1
            k = self.flag % len_

            self.vaccinename.set(items[k][0])
            self.comment.set(items[k][1]['Comments'])
            self.attitude.set(items[k][1]['Attitude'])
            self.willingness.set(items[k][1]['Willingness'])

            for i in self.sideeffects_vars:
                i.set('off')

            if items[k][1]['Side Effects'] != "":
                results = items[k][1]['Side Effects'].split(",")

                for i in range(len(results)):
                    ind = self.sideeffects_choices.index(results[i])
                    self.sideeffects_vars[ind].set("on")

        # elif len(self.result_df["label"][self.index].keys()) == 1:
        #     messagebox.showwarning("警告", "只有一个疫苗！")





    #click on quit to write currect selection to df, and save df to csv file
    def Save(self):
        self.dfResults()
        #writing the resulting datafile to .csv file
        self.results = pd.DataFrame.from_dict(self.result_df, orient= "index").T
        
        
        self.results.to_csv(data_dir+"/"+datafile+"-annot.csv", index = False)   

    #save and quit    
    def Quit(self):
        self.Save()   
        #quit
        self.quit()

    #close window autosave
    def on_closing(self):
        self.Save()
        #if messagebox.askokcancel("Quit", "Do you want to quit?"):
        self.destroy()


if __name__=="__main__":
    datafile = ""
    data_dir = ""
    #prompt dialogue box, same the name of the file and exit
    def file_opener():
        global datafile, data_dir
        input = filedialog.askopenfile(initialdir=os.getcwd(),title="Open File",
            filetypes=[("csv files", "*.csv")])
        datafile = os.path.basename(input.name).split(".")[0]
        data_dir = os.path.dirname(input.name)
        dialogue_box.destroy()
    #dialogue box for file name
    dialogue_box = tk.Tk()
    dialogue_box.geometry('150x150')
    tk.Button(dialogue_box, command=lambda: file_opener(), text='select a file', height = 2, width = 10).pack()
    dialogue_box.title("Open...")
    dialogue_box.mainloop()

    
    #read in data
    df = pd.read_csv(data_dir+"/"+datafile+".csv")
    df = df.fillna("") #get rid of "nan"
    old_col = ["Record", "text"]
    # new_col = [
    # "Advertisement",
    # "Vaccine Name",
    # "Side Effects",
    # "Willingness",
    # "Attitude",
    # "Comments"]
    new_col = [
        "Advertisement",
        "label",]

    df = df[old_col]

    if path.exists(data_dir+"/"+datafile+"-annot.csv"):
        result_df = pd.read_csv(data_dir+"/"+datafile+"-annot.csv")
        for x in old_col:
            result_df[x] =df[x]
        result_df = result_df.fillna("")
    else:
        result_df = df
        
    for x in new_col:
        if x not in result_df.columns:
            result_df[x] = "{}"

    d = result_df.to_dict()

    app = simpleapp_tk(None)
    app.title("Annotation Tool")
    app.mainloop()