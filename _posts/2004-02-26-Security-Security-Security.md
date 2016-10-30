---
layout: post
title: "Security Security Security"
date: 2004-02-26T10:20:00Z
modtime: 2004-02-26T10:20:00Z
pubdate: 2004-02-26T10:20:00Z
originalurl: "http://bloggingabout.net/blogs/rene/default.aspx/blogs/rene/archive/2004/02/26/464.aspx"
---


<p>Ik dacht dat we inmiddels toch slim genoeg waren om een basale beveiliging in te bouwen in een Internet site. Maar bij een code-review van een recent opgeleverd project kwam ik de volgende code tegen:
<br /></p><p><code>public string ValidateUser(string user, string password)
<br />
{
<br />
   string type ;
<br />
   OracleDataReader myReader ;
<br />
   string mySelectQuery = "SELECT TYPE FROM AUTORISATIE WHERE GEBRUIKERSNAAM = '"
<br />
       + user + "' AND WACHTWOORD = '" + password + "'" ;
<br />
    OracleCommand myCommand = new OracleCommand(mySelectQuery,SessieConnectie);
<br />
    myCommand.Connection.Open();
<br />
   myReader = myCommand.ExecuteReader();
<br />
   while (myReader.Read())
<br />
   {
<br />
       type = myReader.GetOracleString(0).ToString() ;
<br />
   }
<br />
   myReader.Close() ;
<br />
   return type;
<br />
}
<br /></code></p><p>Op een login.aspx kan ik de user en password opgeven. Vraag: welke 'user/password' combinatie levert als return 'BEHEERDER' op?</p>
