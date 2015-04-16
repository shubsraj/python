import wx ,base64, hashlib 
from Crypto.Cipher import AES
from Crypto import Random

class windowClass(wx.Frame):
	def __init__(self, *args, **kwargs):
		super(windowClass, self).__init__(*args, **kwargs)
		self.OurGui()
		self.bs = 32
		
	def OurGui(self):
	
		panel = wx.Panel(self)
		menuBar = wx.MenuBar()
		fileButton = wx.Menu()
		encrypttext = wx.Menu()
		AbaoutButton = wx.Menu()
		
		menuBar.Append(fileButton, 'File')
		menuBar.Append(AbaoutButton, 'About')
		
		aboutItem = AbaoutButton.Append(wx.ID_ABOUT, 'About')
		self.SetMenuBar(menuBar)
		self.Bind(wx.EVT_MENU, self.about, aboutItem)
		
		toolbar=self.CreateToolBar()
	
		openToolButton=toolbar.AddLabelTool(wx.ID_ANY, 'Open', wx.Bitmap('fileopen.png'))
		toolbar.Realize()
		self.Bind(wx.EVT_TOOL, self.open, openToolButton)
		
		saveToolButton=toolbar.AddLabelTool(wx.ID_ANY, 'Writer', wx.Bitmap('filesave.png'))
		toolbar.Realize()
		self.Bind(wx.EVT_TOOL, self.Writer, saveToolButton)
		
		enToolButton=toolbar.AddLabelTool(wx.ID_ANY, 'Encryption', wx.Bitmap('lock.png'))
		toolbar.Realize()
		self.Bind(wx.EVT_TOOL, self.encrypt, enToolButton)
		
		deToolButton=toolbar.AddLabelTool(wx.ID_ANY, 'Decryption', wx.Bitmap('unlock.png'))
		toolbar.Realize()
		self.Bind(wx.EVT_TOOL, self.decrypt, deToolButton)
		
		quitToolButton=toolbar.AddLabelTool(wx.ID_ANY, 'Quit', wx.Bitmap('Quit.png'))
		toolbar.Realize()
		self.Bind(wx.EVT_TOOL, self.Quit, quitToolButton)
		
		openItem = fileButton.Append(wx.ID_OPEN, 'Open')
		self.SetMenuBar(menuBar)
		self.Bind(wx.EVT_MENU, self.open, openItem)
		
		saveItem = fileButton.Append(wx.ID_SAVE, 'Save')
		self.SetMenuBar(menuBar)
		self.Bind(wx.EVT_MENU, self.Writer, saveItem)
		
		fileButton.AppendMenu(wx.ID_ANY, 'Encryption', encrypttext)
		encryption = encrypttext.Append(wx.ID_ANY, 'Encrypt')
		decryption = encrypttext.Append(wx.ID_ANY, 'Decrypt')
		self.Bind(wx.EVT_MENU, self.encrypt, encryption)
		self.Bind(wx.EVT_MENU, self.decrypt, decryption)
		
		exitItem = fileButton.Append(wx.ID_EXIT, 'Exit')
		self.SetMenuBar(menuBar)
		self.Bind(wx.EVT_MENU, self.Quit, exitItem)
		
		self.inbox = wx.TextCtrl(panel, pos=(30, 25), size=(300, 300))
		
		
		
		self.SetTitle('Welcome To Text Editer')
		self.Show(True)
		
	def Writer(self, x):
		namBox = wx.TextEntryDialog(None, 'Write desired name of file?', 'File Naming')
		
		if namBox.ShowModal()==wx.ID_OK:
			filename = namBox.GetValue()
			namBox.Destroy()
			data = self.inbox.GetValue()
			target = open(filename+'.txt', 'w')
			target.write(data)
			target.close()
		
	def Quit(self, e):
		self.Close()
		
	def open(self, g):
		openBox = wx.TextEntryDialog(None, 'Enter the name which you wanna open..', 'File Opening..')
		
		if openBox.ShowModal()==wx.ID_OK:
			openname = openBox.GetValue()
			openBox.Destroy()
			file = open(openname+'.txt', 'r')
			dat = file.read()
			set = self.inbox.SetValue(dat)
			file.close()
	
	def encrypt(self, r):
		openBox = wx.TextEntryDialog(None, 'Please enter password', 'Password')
		
		if openBox.ShowModal()==wx.ID_OK:
			password = openBox.GetValue()
			openBox.Destroy()
			self.key = hashlib.sha256(password.encode()).digest()
			message = self.inbox.GetValue()
			raw = self._pad(message)
			iv = Random.new().read(AES.block_size)
			cipher = AES.new(self.key, AES.MODE_CBC, iv)
			ctext = base64.b64encode(iv + cipher.encrypt(raw))
			set = self.inbox.SetValue(ctext)

	def decrypt(self, e):
		openBox = wx.TextEntryDialog(None, 'Please enter password', 'Password')
		
		if openBox.ShowModal()==wx.ID_OK:
			password = openBox.GetValue()
			openBox.Destroy()
			try:
				self.key = hashlib.sha256(password.encode()).digest()
				message = self.inbox.GetValue()
				enc = base64.b64decode(message)
				iv = enc[:AES.block_size]
				cipher = AES.new(self.key, AES.MODE_CBC, iv)
				ptext = self._unpad(cipher.decrypt(enc[AES.block_size:])).decode('utf-8')
				set = self.inbox.SetValue(ptext)
				msg = self.inbox.GetValue()
				if len(msg) == 0:
					samBox = wx.MessageDialog(None, 'Wrong Password!!')
					samAnswer = samBox.ShowModal()
					samBox.Destroy()
					self.inbox.SetValue(message)
			except:
				samBox = wx.MessageDialog(None, 'Your Data Is Already Decrypted')
				samAnswer = samBox.ShowModal()
				samBox.Destroy()
			
	def _pad(self, s):
		return s + (self.bs - len(s) % self.bs) * chr(self.bs - len(s) % self.bs)

	def _unpad(self, s):
		return s[:-ord(s[len(s)-1:])]
		
	def about(self, a):
		samBox = wx.MessageDialog(None, 'This Editor Is Created By Students Of LENP module Of SRV, Kota.'
										'\nIt is An Python Text Editor Made In The Guidance Of Hasan sir'
										'\nBy The Students Shubham, Kunal And Lokesh.'
										'\nHope You Will Enjoy It.')
		samAnswer = samBox.ShowModal()
		samBox.Destroy()
		
			
		
def main():
	app = wx.App()
	windowClass(None)
	app.MainLoop()
	
main()	
