Backend -> Index.js    connection & query file

Terminal : In Backend Folder   -> 
                                 npm i mysql express nodemon cors

Frontend -> App.js     Routing

         -> Test.js    Crud Form ,UseState, UseEffect , Import & and Html code [ Component ]

         1. npx create-react-app react-course
         2. In Frontend Folder  -> 
                                   npm install axios bootstrap react-router-dom



Note:  Axios. request ke URL me Different Type Se kaise Data or id wagera pass karte hai ------->
Frontend : ==> Aise FRONTEND me data pass karenge Toh  `${Base_Url}/requestdatacat/${id}`  
------------
BACKEND me accept karne ka tarika yeh hai

          const edit = async (id) => {
    try {
      const response = await axios.get(`${Base_Url}/requestdatacat/${id}`);
      setFormData(response.data)
      setIsEdit(true);
      console.log(response.data);
    } catch (error) {
      console.error('Error editing user:', error);
    }
  };

Backend:
-------
  app.get('/requestdatacat/:id', (req, res) => {
  
  const { id } = req.params; // URL se user ki id ko extract kar rahe hain
  const sql = "SELECT * FROM awt_category WHERE id = ? AND deleted = 0"; // User ko uske ID aur soft-delete status ke base par fetch karne ki query
  con.query(sql, [id], (err, data) => { // SQL query ko execute kar rahe hain, id ko parameter ke roop me pass kar rahe hain
    if (err) {
      return res.status(500).json(err); // Agar koi error aata hai to 500 status ke sath error message return karenge
    } else {
      return res.json(data[0]); // Agar query successful hoti hai to specific user ka data (index 0) return karenge
    }
  });
});


2nd Method:
FRONTEND -->     `${Base_Url}/deletecatdata`, { id }
----------------------------------------------------------------------
           const deleted = async (id) => {
    try {
      const response = await axios.post(`${Base_Url}/deletecatdata`, { id });
      // alert(response.data[0]);
      window.location.reload();
    } catch (error) {
      console.error('Error deleting user:', error);
    }
  };

BACKEND: ------>  aise value lete hai from url axios.
                        app.post('/deletecatdata', (req, res) => {
          const { id } = req.body; // Request body se user ki ID ko extract kar rahe hain
          const sql = `UPDATE awt_category SET deleted = 1 WHERE id = ?`; // User ko soft-delete karne ki SQL query (deleted column ko 1 kar dena)
          con.query(sql, [id], (err, data) => { // SQL query ko execute kar rahe hain, id ko parameter ke roop me pass kar rahe hain
            if (err) {
              console.error(err); // Agar koi error aata hai to usse console me print karenge
              return res.status(500).json({ message: 'Error updating user' }); // Error message ke sath 500 status return karenge
            } else {
              return res.json(data); // Agar query successful hoti hai to updated data return karenge
            }
          });
        })
