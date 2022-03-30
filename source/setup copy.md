# Setup

## intallation des outils



<table>
    <thead>
        <tr>
            <th align="center" > LOGICIELS   </th>
                   </tr>
    </thead>
    <tbody>
        <tr>
            <td  >PostgreSQL</td>
            <td align="left">port 5432, Base pour le GIC-Server</td>
       </tr>
        <tr>
             <td  >Docker compose</td>
             <td align="left"></td>
        </tr>
          <tr>
             <td  >Reactj (Highchartjs)   </td>
             <td align="left"></td>
        </tr>
          <tr>
             <td  >NodeJs </td>
             <td align="left"></td>
        </tr>
    </tbody>
</table>
	<br/>	
	<br/>	
<table>
    <thead>
        <tr>
            <th align="center" > MARSSVOM1 134.158.16.36   </th>
				<br/>
				<br/>
				<br/>
				<br/>	
                   </tr>
    </thead>
    <tbody>
        <tr>
            <td  ></td>
            <td align="left">Ouverture du port 3000 </td>
       </tr>
        <tr>
             <td  >/usr/local/var/coatli/bin</td>
             <td align="left">Espace de stockage a prévoir </td>
        </tr>
          <tr>
             <td  >data_storeA/coatli </td>
             <td align="left">dark, flat_bank sextractor,</td>
        </tr>
          <tr>
             <td  >data   </td>
             <td align="left"> pipepline, fits</td>
        </tr>
    </tbody>
</table>
 	<br/>	
	<br/>	
<table>
    <thead>
        <tr>
            <th align="center" >marsvom2  134.158.16.37 </th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Ouverture du port 3000 </td>
            <td align="left"></td>
        </tr>
        <tr>
            <td> Espace de stockage </td>
            <td align="left">  a prévoir </td>
        </tr>
        <tr>
            <td>data/ </td>
            <td align="left">GIC WEB, SVOM-DOC,</td>
        </tr>
		  <tr>
            <td>postgresql</td>
            <td align="left">sudo -i -u postgres -->psql --> \c testdb root -->  \dt   <br/>
			sudo npm install pm2@latest -g        <br/>
 pm2 start <br/>
 dans dossier /webApp   <br/>
pm2 start yarn --interpreter zsh --name myapp -- start <br/>
 //ajouter au system <br/>
	  sudo pm2 startup systemd	<br/>	
			   </td>
        </tr>
		  <tr>
            <td>nodejs demarrage  </td>
            <td align="left"> dans dossier /server   </td>
        </tr>
    </tbody>
</table>

