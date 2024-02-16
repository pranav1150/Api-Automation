# Api-Automation
**Fetching data from excel**
import java.io.*
import jxl.*
import jxl.write.*
import java.text.SimpleDateFormat
def tc = testRunner.testCase
tc = testRunner.testCase.testSuite.project
def groovyUtils = new com.eviware.soapui.support.GroovyUtils(context)
def projectroot = groovyUtils.projectPath
def parentpath = new File(projectroot).getParent()
log.info parentpath
Testdata =new File( parentpath +"//Test Data Creation using API//CoreStack_API_TestData.xls")
log.info Testdata
def wk= Workbook.getWorkbook(Testdata)

//************************************************************************************************************************************
//fetching Endpoint and user details from excel
Environment = com.eviware.soapui.SoapUI.globalProperties.getPropertyValue("Environment")

tc.setPropertyValue("Environment",Environment)
def sheet1=wk.getSheet(Environment)
r=sheet1.getRows()

def Environment=[]
def Endpoint=[]
def AccessKey=[]
def SecretKey=[]
def ExistingAccessKey=[]
def ExistingSecretKey=[]
def InternalEndpoint = []

int k=1
for(int j=1;j<r;j++)
{
	
	Cell a1=sheet1.getCell(1,k)
	Environment<<a1.getContents()
	Cell a2=sheet1.getCell(2,k)
	Endpoint<<a2.getContents()
	Cell a3=sheet1.getCell(3,k)
	AccessKey<<a3.getContents()
	Cell a4=sheet1.getCell(4,k)
	SecretKey<<a4.getContents()
	Cell a5=sheet1.getCell(5,k)
	ExistingAccessKey<<a5.getContents()
	Cell a6=sheet1.getCell(6,k)
	ExistingSecretKey<<a6.getContents()
	Cell a7 = sheet1.getCell(7, k)
	InternalEndpoint<<a7.getContents()
	k++
}

//Cell a8 = sheet1.getCell(33,1)
//def MSPuser_accesskey =a8.getContents()
//log.info MSPuser_accesskey
// 
//Cell a9 = sheet1.getCell(34,1)
//def MSPuser_secretkey =a9.getContents()
//log.info MSPuser_secretkey

Run_Environment=tc.getPropertyValue("Environment")
log.info "Script Executing in "+ Run_Environment + " Environment"
index=Environment.indexOf(Run_Environment)

//tc.setPropertyValue("MSPuser_accesskey",MSPuser_accesskey)
//tc.setPropertyValue("MSPuser_secretkey",MSPuser_secretkey)
tc.setPropertyValue("endpoint",Endpoint[index])
//log.info Endpoint[index] + "adfbalksbfasdf"
tc.setPropertyValue("AccessKey",AccessKey[index])
tc.setPropertyValue("SecretKey",SecretKey[index])
tc.setPropertyValue("Acceskey_existing_user",ExistingAccessKey[index])
tc.setPropertyValue("Secretkey_existing_user",ExistingSecretKey[index])
//tc.setPropertyValue("InternalEndpoint", InternalEndpoint[index])

//log.info "EndPoint: "+Endpoint[index]
//log.info "AccountAdmin_username "+AccessKey[index]
//log.info "AccountAdmin_Password "+SecretKey[index]
//log.info "Acceskey_existing_user"+ExistingAccessKey[index]
//log.info "Secretkey_existing_user"+ExistingSecretKey[index]

//************************************************************************************************************//
//Fetching Create Service Account request from excel

sheet1=wk.getSheet("CloudAccountDetails")
r=sheet1.getRows()

//def str = [
//	"azure_account_id_account_scope", "azure_account_name_account_scope",
//	"azure_account_id_tenant_scope", "azure_account_name_tenant_scope"
//]
//
//def init = 0
//for(def i = 43 ; i < 45 ; i ++)
//{
//
//	for(def j = 2 ; j <= 3 ; j ++)
//	{
//		log.info i + " " +  j
//		Cell a01=sheet1.getCell(j, i)
//		data = a01.getContents()
//		log.info data
//		tc.setPropertyValue(str[init], data)
//		init ++
//	}
//}
//Get one Azure cloud account and use it for CMDB
def GET_CLOUD_ACCONT_FLAG = 0
for(def i = 0 ; i < 40 ; i ++)
{
	Cell a01= sheet1.getCell(0, i)
	cloud_name = a01.getContents().toLowerCase()
	Cell a02 = sheet1.getCell(1, i)
	cloud_type = a02.getContents().toLowerCase()
	if(cloud_name == "azure" && cloud_type == "pay as you go")
	{
//		log.info "mil gahasdfadsfdasf"
		GET_CLOUD_ACCONT_FLAG = 1
		Cell a03 = sheet1.getCell(2, i)
		account_id = a03.getContents()
		Cell a04 = sheet1.getCell(3, i)
		account_name = a04.getContents()
		tc.setPropertyValue("azure_account_id_account_scope", account_id)
		tc.setPropertyValue("azure_account_name_account_scope", account_name)
		break
	}
}

if(GET_CLOUD_ACCONT_FLAG == 0)
{
	testRunner.fail "No Azure Pay as you Go accounts are there in excel sheet"
}

//tc.setPropertyValue("internal_endpoint", InternalEndpoint[index])
//tc.setPropertyValue("internal_endpoint_2", InternalEndpoint[index])
//InternalEndpoint[index] += "18080/"
def internal = InternalEndpoint[index]
//log.info internal
def index = internal.lastIndexOf(":")
//log.info index
def just_internal = internal.substring(0, index + 1)
//log.info just_internal 
just_internal_4 = just_internal + "18083/"
just_internal_3 = just_internal + "18087/"
just_internal_2 = just_internal + "18092/"
just_internal += "18080/"
//log.info just_internal
tc.setPropertyValue("internal_endpoint_QA_4", just_internal_4)
tc.setPropertyValue("internal_endpoint_QA_3", just_internal_3)
tc.setPropertyValue("internal_endpoint_QA_2", just_internal_2)
tc.setPropertyValue("internal_endpoint_QA", just_internal)
//tc.setPropertyValue("Endpoint",Endpoint[index])

//log.info "Deleting AWS master account: " +a1.getContents()
//log.info "Deleting Azure EA account: " +a2.getContents()
//log.info "Deleting Azure Enterprise Account: " +a3.getContents()
//log.info "Deleting GCP Account: " +a4.getContents()

