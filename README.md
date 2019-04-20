# 简单爬虫
# 根据xpath获取节点内容：

getNodesTxt <- function(html_txt1,xpath_p){

  els1 = getNodeSet(html_txt1, xpath_p)

  # 获得Node的内容，并且去除空字符：

  els1_txt <- sapply(els1,xmlValue)[!(sapply(els1,xmlValue)=="")]

  # 去除\n：

  str_replace_all(els1_txt,"(\\n )+","")

}



# 处理节点格式，为character且长度为0的赋值为NA：

dealNodeTxt <- function(NodeTxt){

  ifelse(is.character(NodeTxt)==T && length(NodeTxt)!=0 , NodeTxt , NA)

}



for(i in 1:nrow(mydata)){

  # 获得网址：

  doc <- getURL(mydata[i,"url"])

  cat("成功获得网页!\t")

  # 获得网页内容

  html_txt1 = htmlParse(doc, asText = TRUE)

  # 获得Full Name:

  mydata[i,"name"] <- dealNodeTxt(getNodesTxt(html_txt1,'Xpath'))

  cat("写入名称!\t")

  print(paste("完成第",i,"个了！"))

}

write.csv(mydata,"mydata.csv")
