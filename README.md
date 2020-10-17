# project-1

const fs = require('fs');
const express = require("express");
const app = express();
app.use(express.json());

app.use((req, res, next) => {
    console.log('hello from middleware')
    next();
});

app.use((req, res, next) => {
    req.requestTime = new Date(). toISOString();
    next();
})

// app.get('/', (req,res) =>{
//     res 
//       .status(200)
//       .json({message: 'hello from the server side', app: 'natours'});
// })

const tours = JSON.parse(
    fs.readFileSync(`${__dirname}/starter/dev-data/data/tours-simple.json`)
);


// ////////////////////
// // To get all code from database to Postman we do like this 
// ///////////////////



// app.get(`/api/v1/tours`, (req,res) => {
//     res.status(200).json({
//         status: 'success',
//         results: tours.length,
//         data: {
//             tours
//         }
//     })
// });


// ////////////////////
// // To get all code and get value by filter by id  from database to Postman we do like this 
// ///////////////////


// app.get('/api/v1/tours/:id', (req,res) => {
//     console.log(req.params);

//     const id = req.params.id * 1;
//     const tour = tours.find(el=> el.id ===id);

//     if(id> tours.length){
//         return res.status(404).json({
//             status: 'fail',
//             message: 'File not found',
//         })
//     }

//     res.status(200).json({
//         status: 'success',
//         data: {
//             tour
//         }
//     })
// })


// ////////////////////
// // To get a sepecific code from database to Postman we do like this 
// ///////////////////


// app.post('/api/v1/tours/:id', (req, res) => {
//     // console.log(req.body);
//     const newid = tours[tours.length - 1].id + 1;
//     const newTour = Object.assign({id : newid }, req.body);
  
//     tours.push(newTour);
//     fs.writeFile(`${__dirname}/starter/dev-data/data/tours-simple.json`, JSON.stringify(tours), err =>{
//     res.status(201).json({
//         status: 'success',
//         data: {
//             tour: newTour
//         }
//     });
//     });
// })

// ////////////////////
// // To update some code in Postman we do like this 
// ///////////////////

// app.patch('/api/v1/tours/:id', (req,res) => {

//     if(req.params.id * 1 > tours.length){
//         return res.status(404).json({
//             status: 'fail',
//             message: 'File not found',
//         });
//     }

//     res.status(200).json({
//         status: 'success',
//         data: {
//             tours: '<updated tour is here>....'
//         }
//     })
// })

// ////////////////////
// // To delete some code in Postman we do like this 
// ///////////////////

// app.delete('/api/v1/tours/:id', (req,res) => {

//     if(req.params.id * 1 > tours.length){
//         return res.status(404).json({
//             status: 'fail',
//             message: 'File not found',
//         });
//     }

//     res.status(204).json({
//         status: 'success',
//         data: null
//     })
// })



const port = 3000;
app.listen(port, () => {
    console.log(`app running on port ${port}....`)
})










//////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
// Re-Structure Whole Abve Code
//////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////












////////////////////
// To get all tours from file to postman
///////////////////

const getalltours = (req,res) => {
    console.log(req.requestTime)
    res.status(200).json({
        requestedAt: req.requestTime,
        status: 'success',
        results: tours.length,
        data: {
            tours
        }
    })
};

////////////////////
// To get tour from file to postman
///////////////////

const getTour = (req,res) => {
    console.log(req.params);

    const id = req.params.id * 1;
    const tour = tours.find(el=> el.id ===id);

    if(id> tours.length){
        return res.status(404).json({
            status: 'fail',
            message: 'File not found',
        })
    }

    res.status(200).json({
        status: 'success',
        data: {
            tour
        }
    })
}

////////////////////
// To create a tour
///////////////////

const createTour = (req, res) => {
    // console.log(req.body);
    const newid = tours[tours.length - 1].id + 1;
    const newTour = Object.assign({id : newid }, req.body);
  
    tours.push(newTour);
    fs.writeFile(`${__dirname}/starter/dev-data/data/tours-simple.json`, JSON.stringify(tours), err =>{
    res.status(201).json({
        status: 'success',
        data: {
            tour: newTour
        }
    });
    });
}

////////////////////
// To update a tour 
///////////////////

const updateTour = (req,res) => {

    if(req.params.id * 1 > tours.length){
        return res.status(404).json({
            status: 'fail',
            message: 'File not found',
        });
    }

    res.status(200).json({
        status: 'success',
        data: {
            tours: '<updated tour is here>....'
        }
    })
}

////////////////////
// To delete some code in Postman we do like this 
///////////////////

const deleteTour = (req,res) => {

    if(req.params.id * 1 > tours.length){
        return res.status(404).json({
            status: 'fail',
            message: 'File not found',
        });
    }

    res.status(204).json({
        status: 'success',
        data: null
    })
}



// app.get(`/api/v1/tours`, getalltours);

// app.get('/api/v1/tours/:id', getTour)

// app.post('/api/v1/tours/:id', createTour)

// app.patch('/api/v1/tours/:id',updateTour )

// app.delete('/api/v1/tours/:id', deleteTour)


app 
.route(`/api/v1/tours`)
.get(getalltours);

app
.route('/api/v1/tours/:id')
.get(getTour)
.post(createTour)
.patch(updateTour)
.delete(deleteTour);



