# Python
Projects for Pace University
__author__ = 'naveen123'

import tkinter
from tkinter import *
from tkinter import ttk

global i, b, c, sem_list, d, subcon, semcount, semname, semyear, subject, subjinc, subjcode, subjname, cha, chp, gr, gp, w, first, last, sd, ssn, w,deg, major, matric, sex
semyear = [''] * 14
semname = [''] * 14
subcon = [''] * 14
subject = [''] * 100
subjcode = [''] * 100
subjname = [''] * 100
cha = [''] * 100
chp = [''] * 100
gr = [''] * 100
gp = [''] * 100
semcount = -1
subjinc = -1


class AutocompleteEntry(tkinter.Entry):
    """
       D Subclass of tkinter.Entry that features autocompletion.
      To enable autocompletion use set_completion_list(list) to define   H
       R a list of possible strings to hit.
        To cycle through hits use down and up arrow keys. U
        V  Autocomplete help
        """

    def set_completion_list(self, completion_list):
        self._completion_list = sorted(completion_list, key=str.lower)  # Work with a sorted list
        self._hits = []
        self._hit_index = 0
        self.position = 0
        self.bind('<KeyRelease>', self.handle_keyrelease)

    def autocomplete(self, delta=0):
        """autocomplete the Entry, delta may be 0/1/-1 to cycle through possible hits"""
        if delta:  # need to delete selection otherwise we would fix the current position
            self.delete(self.position, tkinter.END)
        else:  # set position to end so selection starts where textentry ended
            self.position = len(self.get())
        # collect hits
        _hits = []
        for element in self._completion_list:
            if element.lower().startswith(self.get().lower()):  # Match case-insensitively
                _hits.append(element)
        # if we have a new hit list, keep this in mind
        if _hits != self._hits:
            self._hit_index = 0
            self._hits = _hits
        # only allow cycling if we are in a known hit list
        if _hits == self._hits and self._hits:
            self._hit_index = (self._hit_index + delta) % len(self._hits)
        # now finally perform the auto completion
        if self._hits:
            self.delete(0, tkinter.END)
            self.insert(0, self._hits[self._hit_index])
            self.select_range(self.position, tkinter.END)

    def handle_keyrelease(self, event):
        """event handler for the keyrelease event on this widget"""
        if event.keysym == "BackSpace":
            self.delete(self.index(tkinter.INSERT), tkinter.END)
            self.position = self.index(tkinter.END)
        if event.keysym == "Left":
            if self.position < self.index(tkinter.END):  # delete the selection
                self.delete(self.position, tkinter.END)
            else:
                self.position = self.position - 1  # delete one character
                self.delete(self.position, tkinter.END)
        if event.keysym == "Right":
            self.position = self.index(tkinter.END)  # go to end (no selection)
        if event.keysym == "Down":
            self.autocomplete(1)  # cycle to next hit
        if event.keysym == "Up":
            self.autocomplete(-1)  # cycle to previous hit
        if len(event.keysym) == 1:
            self.autocomplete()


class AutocompleteCombobox(ttk.Combobox):
    def set_completion_list(self, completion_list):

        self._completion_list = sorted(completion_list, key=str.lower)
        self._hits = []
        self._hit_index = 0
        self.position = 0
        self.bind('<KeyRelease>', self.handle_keyrelease)
        self['values'] = self._completion_list

    def autocomplete(self, delta=0):

        if delta:
            self.delete(self.position, tkinter.END)
        else:
            self.position = len(self.get())

        _hits = []
        for element in self._completion_list:
            if element.lower().startswith(self.get().lower()):
                _hits.append(element)

        if _hits != self._hits:
            self._hit_index = 0
            self._hits = _hits

        if _hits == self._hits and self._hits:
            self._hit_index = (self._hit_index + delta) % len(self._hits)

        if self._hits:
            self.delete(0, tkinter.END)
            self.insert(0, self._hits[self._hit_index])
            self.select_range(self.position, tkinter.END)

    def handle_keyrelease(self, event):

        if event.keysym == "BackSpace":
            self.delete(self.index(tkinter.INSERT), tkinter.END)
            self.position = self.index(tkinter.END)
        if event.keysym == "Left":
            if self.position < self.index(tkinter.END):
                self.delete(self.position, tkinter.END)
            else:
                self.position = self.position - 1
                self.delete(self.position, tkinter.END)
        if event.keysym == "Right":
            self.position = self.index(tkinter.END)
        if len(event.keysym) == 1:
            self.autocomplete()
def callsem():
    global i, b, c, sem_list, d, subcon, semcount, semname, semyear, subject, subjinc, subjcode, subjname, cha, chp, gr, gp

    # destroy buttons to add new content
    b.destroy()
    c.destroy()
    d.destroy()

    semcount += 1
    subjinc +=1
    i += 1

    Label(master, text="Semester").grid(row=i, column=1)
    semname[semcount] = AutocompleteEntry(master)
    semname[semcount].set_completion_list(sem_list)
    semname[semcount].grid(row=i, column=2)
    semname[semcount].focus_set()
    semyear[semcount] = Entry(master)
    semyear[semcount].grid(row=i, column=3)
    Label(master, text="Number of Subjects").grid(row=i, column=4)
    subcon[semcount] = Entry(master)
    subcon[semcount].grid(row=i, column=5)

    i += 1
    Label(master, text="Course id").grid(row=i, column=1)
    Label(master, text="Course name").grid(row=i, column=2)
    Label(master, text="Credit Hours Attempted").grid(row=i, column=3)
    Label(master, text="Credit Hours Passed").grid(row=i, column=4)
    Label(master, text="Grade").grid(row=i, column=5)
    Label(master, text="Quality Points").grid(row=i, column=6)
    i += 1

    subjcode[subjinc] = AutocompleteEntry(master)
    subjcode[subjinc].set_completion_list(code_list)
    subjcode[subjinc].grid(row=i, column=1, padx=2, pady=2)
    subjcode[subjinc].focus_set()

    subjname[subjinc] = AutocompleteEntry(master)
    subjname[subjinc].set_completion_list(subject_list)
    subjname[subjinc].grid(row=i, column=2, padx=2, pady=2)
    subjname[subjinc].focus_set()

    cha[subjinc] = Entry(master)
    cha[subjinc].grid(row=i, column=3, padx=2, pady=2)
    chp[subjinc] = Entry(master)
    chp[subjinc].grid(row=i, column=4, padx=2, pady=2)
    gr[subjinc] = Entry(master)
    gr[subjinc].grid(row=i, column=5, padx=2, pady=2)
    gp[subjinc] = Entry(master)
    gp[subjinc].grid(row=i, column=6, padx=2, pady=2)
    i += 1
    c = Button(master, text="Add Subject", command=callback)
    c.grid(row=i, column=1, padx=2, pady=2)
    b = Button(master, text="Add Semester", command=callsem)
    b.grid(row=i, column=2, padx=2, pady=2)
    d = Button(master, text="Enter Final Detail", command=calldegree)
    d.grid(row=i, column=4, padx=2, pady=2)


# to add subjects in current semester
def callback():
    global i, b, c, sem_list, d, subcon, semcount, semname, semyear, subject, subjinc, subjcode, subjname, cha, chp, gr, gp

    b.destroy()
    c.destroy()
    d.destroy()
    i += 1
    subjinc += 1
    subjcode[subjinc] = AutocompleteEntry(master)
    subjcode[subjinc].set_completion_list(code_list)
    subjcode[subjinc].grid(row=i, column=1, padx=2, pady=2)
    subjcode[subjinc].focus_set()

    subjname[subjinc] = AutocompleteEntry(master)
    subjname[subjinc].set_completion_list(subject_list)
    subjname[subjinc].grid(row=i, column=2, padx=2, pady=2)
    subjname[subjinc].focus_set()

    cha[subjinc] = Entry(master)
    cha[subjinc].grid(row=i, column=3, padx=2, pady=2)
    chp[subjinc] = Entry(master)
    chp[subjinc].grid(row=i, column=4, padx=2, pady=2)
    gr[subjinc] = Entry(master)
    gr[subjinc].grid(row=i, column=5, padx=2, pady=2)
    gp[subjinc] = Entry(master)
    gp[subjinc].grid(row=i, column=6, padx=2, pady=2)
    i += 1
    c = Button(master, text="Add Subject", command=callback)
    c.grid(row=i, column=1, padx=2, pady=2)
    b = Button(master, text="Add Semester", command=callsem)
    b.grid(row=i, column=2, padx=2, pady=2)
    d = Button(master, text="Enter Final Detail", command=calldegree)
    d.grid(row=i, column=4, padx=2, pady=2)


# to enter final details about the degree
def calldegree():
    global i, b, c, sem_list, d, subcon, semcount, semname, semyear, subject, subjinc, subjcode, subjname, cha, chp, gr, gp, da, datee, remark
    b.destroy()
    c.destroy()
    d.destroy()
    i += 1
    Label(master, text="Degree Awarded").grid(row=i, column=1)
    da = Entry(master)
    da.grid(row=i, column=2)
    Label(master, text="Date").grid(row=i, column=3)
    datee = Entry(master)
    datee.grid(row=i, column=4)
    i += 1
    Label(master, text="Remarks").grid(row=i, column=1)
    remark = Entry(master)
    remark.grid(row=i, column=2, sticky='w')
    i += 1
    gen = Button(master, text="Generate Transcript", command=generate)
    gen.grid(row=i, column=2)


def generate():
    global i, b, c, sem_list, d, subcon, semcount, semname, semyear, subject, subjinc, subjcode, subjname, cha, chp, gr, gp, w, first, last, sd, ssn, w,deg, major, matric, sex, variable
    firstname = first.get()
    lastname = last.get()
    dob = sd.get()
    social = ssn.get()
    campus = variable.get()
    degree = deg.get()
    maajor = major.get()
    matri = matric.get()
    gender = sex.get()

    z = semcount

    t=0



    transfile = open("trans.txt", "w")
    transfile.write("first Name :"+ firstname + "\t\tLast Name :" + lastname + "\n Date of Birth :" + dob + "\n Social Security :" + social + "\n Campus :" + campus + "\n Degree :" + degree + "\n Major :" + maajor + "\n Matric :" + matri + "\n Gender :" + gender + "\n\n" )
    if z==0 :
        gpppp = 0
        gradeing= 0
        semnamee = semname[0].get()
        semyearr = semyear[0].get()
        subCcount= subcon[0].get()
        transfile.write("Semester :" + semnamee + semyearr + "\n")
        for q in range(0, int(subCcount)):
            subcode=subjcode[t].get()
            subname=subjname[t].get()
            chaa=cha[t].get()
            chpp=chp[t].get()
            grr= gr[t].get()
            gpp = gp[t].get()
            gppp = float(gpp)
            gradeing += gppp
            t += 1
            gpppp +=1
            transfile.write("Code :" + subcode + " \t Subject :" + subname + "\tCredit Hours Attempted :" + chaa + "\t Credit Hours Passed :" + chpp + "\t Grade: " + grr + "\t Quality Points :" + gpp + "\n" )
        gpa = (gradeing/(gpppp*3))
        transfile.write("Semster :"+ semnamee + semyearr + "\t GPA : " + str(gpa) + "\n\n")
        daa= da.get()
        dateeee = datee.get()
        remarkss= remark.get()
        transfile.write("Degree Awarded : " + daa + "\n Date : " + dateeee + "\t Remarks :" + remarkss)
        transfile.close()
    else:
        for x in range(0, z+1):
            gpppp = 0
            gradeing= 0
            semnamee = semname[x].get()
            semyearr = semyear[x].get()
            subCcount= subcon[x].get()
            transfile.write("Semester :" + semnamee + semyearr + "\n")
            for q in range(0, int(subCcount)):
                subcode=subjcode[t].get()
                subname=subjname[t].get()
                chaa=cha[t].get()
                chpp=chp[t].get()
                grr= gr[t].get()
                gpp = gp[t].get()
                gppp = float(gpp)
                gradeing += gppp
                t += 1
                gpppp +=1
                transfile.write("Code :" + subcode + "\t Subject :" + subname + " \t Credit Hours Attempted : " + chaa + "\t Credit Hours Passed :" + chpp + "\t Grade :" + grr + "\t Quality Points : " + gpp + "\n" )
            gpa = (gradeing/(gpppp*3))
            transfile.write("Semster : "+ semnamee + semyearr + "\t GPA : " + str(gpa) + "\n\n")
        daa= da.get()
        dateeee = datee.get()
        remarkss= remark.get()
        transfile.write("Degree Awarded :" + daa + "\n Date :" + dateeee + "\t Remarks :" + remarkss)
        transfile.close()



# Subject list
subject_list = (
    'THEORY OF ACC 1', 'THEORY OF ACC 2', 'INTRO TO FIN ACC', 'INTRO MANAGRL ACC', 'INTERMEDIATE ACC',
    'INTERMEDIATE ACC',
    'INTERMEDIATE ACC 1', 'COST ACCOUNTING', 'SPECIALIZED ACC 1', 'SPECIALIZED ACC 2', 'INTERNAL AUDITING',
    'PROF AUDITING 1', 'PROF AUDITING 2', 'ADV ACC PRACTICE', 'SEMINAR IN ACC 1', 'SEMINAR IN ACC 2',
    'INTRO TO THE ARTS',
    'HIST OF ARCHITECTURE', 'HIST & APP FINE ARTS', 'ORIENTAL ART', 'PAINT & SKETCH 1', 'PAINT & SKETCH 2',
    'MUSIC APPRECIATION', 'ANATOMY & PHYS 1', 'ANATOMY & PHYS 2', 'BASIC MICROBIOLOGY', 'INTRO TO COMPUTING',
    'INTRO TO COMP SCI', 'COBOL PROGRAMMING', 'PRIN OF ECO 1', 'PRIN OF ECO 2', 'MONEY & BANKING', 'ELEC DATA PROC',
    'READING IMPRVMNT', 'BE EFFICIENT READER', 'WRITING WORKSHOP', 'FUND OF ENGLISH', 'ENGLISH 1', 'ENGLISH 2',
    'BUSINESS ENGLISH', 'MSTRS ENG LIT 1', 'MASTERS OF LIT 1', 'MASTERS OF LIT 2', 'MSTRS ENG LIT 2', 'JOURNALISM',
    'PRIN OF FIN ANALYSIS ‘, ‘FIN PROBS CORP ENT 1', 'FIN PROBS CORP ENT 2', 'ADV FIN ANALYSIS', 'INTRO TO SOC SCI',
    'HIST OF CIV 2', 'AMER CIV TO 1900', 'AMER CIV TO 1877', 'AMER CIV SINCE 1877', 'EUROPE 14 TO 18 CENT',
    'MODERN EUROPE',
    'HIST MYTHS', 'COMMERCIAL LAW 1', 'COMMERCIAL LAW 2', 'COMMERCIAL LAW 3', 'SEMINAR IN ACC & LAW', 'BASIC MARKETING',
    'INTRO MANAGRL MKTG', 'BROADCSTNG PRIN MKTG', 'MKTG RESEARCH', 'COMPUTERS IN MKTG ‘, ‘ADV MKTG MANAGEMENT',
    'ALGEBRA',
    'FINITE MATH', 'ELEM CALCULUS', 'BASIC CONCPTS MATH', 'ELEM CALCULUS 1', 'ELEM STATISTICS', 'BUSINESS & SOCIETY',
    'BUSINESS POLICIES', 'ADV BUS POLICIES', 'MGT SIMULATION MODEL', 'INTRO TO NURSING', 'MAT & CHILD HLTH NUR',
    'HIST OF PHILOSOPHY', 'LOGIC', 'GEN PSYCHOLOGY 1', 'GEN PSYCHOLOGY 2', 'ASTRONOMY', 'OCEANOGRAPHY',
    'INTRO TO SOCIOLOGY', 'FUND OF SPEECH', 'FUND OF SPEAKING', 'INTERPERSONAL COMM', 'FED INC TAX LAW 1',
    'FED INC TAX LAW 2', 'STATE & MUN TAX', 'WRITING WORKSHOP', 'OCL', 'THEORY OF ACC 1', 'LAW PRIN CONTRCTS',
    'LAW AGCY PARTSHP', 'LAW CORP NEG INST', 'LAW SALE/BAIL/INS/SUR', 'LAW FIDUCIARIES', 'FED INC TAX LAW 1',
    'ADV FEDERAL TAX', 'STATE & MUN TAX', 'COMMERCIAL LAW 1 ‘, ‘BUSINESS LAW 1', 'COMMERCIAL LAW 2',
    'FUNDMTLS AUDITING',
    'THEORY OF ACC 2', 'PREP MATH 2', 'COLLEGE MATH 1', 'COLLEGE ALGEBRA', 'COLLEGE MATH 2', 'MATH OF BUS & FIN',
    'FINITE MATH', 'COLLEGE MATH 3', 'ELEM CALCULUS', 'STATISTICS 1', 'STATISTICS 2', 'ELEM STATISTICS',
    'BASIC CONC MATH 2', 'LINEAR MATH ‘, ‘LINEAR ALGEBRA', 'MODERN GEOMETRY 1', 'ASTRONOMY', 'INTRO PHYSICAL SCI',
    'INTRO BIO SCIENCE', 'CORPORATION ACC', 'PRIN CRED & COLL', 'CORP FINANCE', 'MANAGRL ECO & FIN',
    'BUSINESS POLICIES',
    'OFFICE MANAGEMENT', 'INTERMEDIATE ACC', 'SYSTMS & PROCEDRS', 'ENGLISH 1', 'ENGLISH 2', 'COMMUNICATION 1A',
    'COMMUNICATION 1B', 'COMMUNICATION 1', 'COMMUNICATION 2', 'MSTRS ENG LIT 1', 'MSTRS ENG LIT 2', 'PUBLIC SPEAKING',
    'WRIT & ORAL REPORTS', 'COST ACCOUNTING', 'HISTORY OF CIV 1', 'HISTORY OF CIV 2', 'PRIN OF ECO 1', 'PRIN OF ECO 2',
    'MONEY & BANKING', 'AMER CIV & GOVT 1', 'AMER CIV & GOVT 2', 'AMER CIV SINCE 1900', 'BUS CYCLES & FORE',
    'LABOR ECONOMICS', 'PRIN PSYCHOLOGY 1', 'PRIN PSYCHOLOGY 2', 'LOGIC', 'HIST OF PHILOSOPHY', 'GENERAL ETHICS',
    'HISTORY OF IDEAS', 'SOCIOLOGY', 'PRIN OF PSY 1', 'PRIN OF PSY 2', 'SOCIAL PSYCHOLOGY', 'INTRO TO THE ARTS',
    'PAINT & SKETCH 2', 'SPECIALIZED ACC 1', 'PRIN OF MARKETING', 'PRIN OF MKTG 1', 'BASIC MARKETING', 'PRIN OF MKTG 2',
    'MKTG CHANNEL DIST', 'SALES PROMOTION', 'SALES MANAGEMENT', 'BEHAV SCI IN MKTG', 'INDUSTRIAL MKTG',
    'PUBLIC RELATION',
    'SPECIALIZED ACC 2', 'FUND OF SPEECH', 'FUND OF SPEAKING', 'COLLEGE TYPING', 'SPEED DICTATION', 'ELEM FRENCH 1',
    'INT DISC CONF')
# Course ID List
code_list = (
    'ACC101 D', 'ACC102 D', 'ACC103 D', 'ACC104 D', 'ACC105 D', 'ACC105 D', 'ACC106 D', 'ACC108 D', 'ACC231 D',
    'ACC232 D',
    'ACC240 D', 'ACC243 D', 'ACC244 D', 'ACC245 D', 'ACC248 D', 'ACC249 D', 'ART101 D', 'ART104 D', 'ART106 D',
    'ART110 D',
    'ART112 D', 'ART113 D', 'ART141 D', 'BIO152 D', 'BIO153 D', 'BIO154 D', 'CIS101 D', 'CIS101 D', 'CIS102 D',
    'ECO105 D',
    'ECO106 D', 'ECO238 D', 'EDP101 D', 'EDU164 D', 'EDU165 D', 'ENG071 D', 'ENG100 D', 'ENG101 D', 'ENG102 D',
    'ENG106 D',
    'ENG111 D', 'ENG111 D', 'ENG112 D', 'ENG112 D', 'ENG124 D', 'FIN201 D', 'FIN205 D', 'FIN206 D', 'FIN220 D',
    'HIS100 D',
    'HIS102 D', 'HIS111 D', 'HIS111 D', 'HIS112 D', 'HIS122 D', 'HIS123 D', 'HIS200 D', 'LAW101 D', 'LAW102 D',
    'LAW103 D',
    'LAW249 D', 'MAR101 D', 'MAR103 D', 'MAR122 D', 'MAR215 D', 'MAR248 D', 'MAR281 D', 'MAT103 D', 'MAT104 D',
    'MAT105 D',
    'MAT106 D', 'MAT111 D', 'MAT117 D', 'MGT102 D', 'MGT201 D', 'MGT202 D', 'MGT258 D', 'NUR101 D', 'NUR102 D',
    'PHI101 D',
    'PHI104 D', 'PSY100 D', 'PSY101 D', 'SCI121 D', 'SCI142 D', 'SOC102 D', 'SPE101 D', 'SPE102 D', 'SPE104 D',
    'TAX101 D',
    'TAX102 D', 'TAX103 D', '1D', '101D', '102D', '103D', '104D', '113D', '116D', '117D', '118D', '136D', '136D',
    '137D',
    '17D', '2D', '202D', '203D', '203D', '204D', '204D', '204D', '205D', '205D', '207D', '208D', '209D', '217D', '218D',
    '239D', '246D', '259D', '281D', '282D', '3D', '308D', '333D', '339D', '354D', '360D', '4D', '40D', '401D', '402D',
    '405D', '405D', '405D', '406D', '416D', '417D', '425D', '469D', '5D', '500D', '501D', '505D', '506D', '526D',
    '545D',
    '546D', '548D', '564D', '568D', '603D', '604D', '616D', '617D', '618D', '619D', '662D', '670D', '671D', '677D',
    '690D',
    '696D', '7D', '700D', '700D', '701D', '701D', '708D', '710D', '712D', '715D', '723D', '760D', '8D', '800D', '801D',
    '850D', '865D', '901D', 'D')
# Semester List
sem_list = ('FALL', 'SPRING', 'SUMMER I', 'SUMMER II')

def dikhao():

    global i, b, c, sem_list, d, subcon, semcount, semname, semyear, subject, subjinc, subjcode, subjname, cha, chp, gr, gp, w, first, last, sd, ssn, variable,deg, major, matric, sex
    Label(master, text="First Name").grid(row=0, column=1)
    Label(master, text="Last Name").grid(row=0, column=2)
    Label(master, text="Date Of Birth").grid(row=0, column=3)
    Label(master, text="SSN").grid(row=0, column=4)
    Label(master, text="Campus").grid(row=0, column=5)

    first = Entry(master)
    first.grid(row=1, column=1)

    last = Entry(master)
    last.grid(row=1, column=2)
    sd = Entry(master)
    sd.grid(row=1, column=3)
    ssn = Entry(master)
    ssn.grid(row=1, column=4)
    variable = StringVar(master)
    variable.set("one")

    w = OptionMenu(master, variable, "NYC", "PLV", "BC")
    w.grid(row=1, column=5)

    Label(master, text="Degree").grid(row=0, column=6)
    Label(master, text="Major").grid(row=0, column=7)
    Label(master, text="Matric").grid(row=0, column=8)
    Label(master, text="Sex").grid(row=0, column=9)
    deg = Entry(master)
    deg.grid(row=1, column=6)
    major = Entry(master)
    major.grid(row=1, column=7)
    matric = Entry(master)
    matric.grid(row=1, column=8)
    sex = Entry(master)
    sex.grid(row=1, column=9)


    # grid variable
    i = 4









    b = Button(master, text="Add Semester", command=callsem)
    b.grid(row=(i + 1), column=2)
    c = Button(master, text="Add Subject", command=callback)
    c.grid(row=(i + 1), column=3)
    d = Button(master, text="Enter Final Detail", command=calldegree)
    d.grid(row=(i + 1), column=4)

master = Tk()

# frame size
master.geometry('1024x800')
# window title
master.title("TRANSCRIPTS")
# student details
dikhao()

mainloop()
# the main app
master.mainloop()

