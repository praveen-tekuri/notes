## 1. Creating a Schema

> User Schema

```js
// models/user.js

const mongoose = require("mongoose");

const userSchema = new mongoose.Schema({
    login: { type: String, required: true, unique: true, trim: true },
    password: { type: String, required: true },
    age: { type: Number, required: true, min: 0 },
    isStudent: { type: Boolean, required: true },
    country: { type: String, required: true, trim: true }
});

const User = mongoose.model('User', userSchema, "users");

module.exports = { User };

// Note: Adding unique: true to login prevents two users from registering with the exact same username.
```

> Country Schema

```js
// models/country.js

const mongoose = require("mongoose");

const countrySchema = new mongoose.Schema({
    country: { type: String, required: true, trim: true },
    capital: { type: String, required: true, trim: true },
    area: { type: Number, required: true },
    population: { type: Number, required: true },
    currency: { type: String, required: true, uppercase: true, trim: true }
});

const Country = mongoose.model("Country", countrySchema, "countries");

module.exports = { Country };
```

## 2. Database Connection

```js
// database.js 

const mongoose = require('mongoose');
const { MongoMemoryServer } = require('mongodb-memory-server');

const connectDB = async() => {
  try {
    // 1. Create an in-memory MongoDB instance
    const mongod = await MongoMemoryServer.create();
    const uri = mongod.getUri();

    // 2. Connect Mongoose to the ephemeral in-memory URI
    await mongoose.connect(uri);
    console.log(`Connected to In-Memory MongoDB successfully at: ${uri}`);
  } catch (error) {
    console.error('Database connection failed:', error);
    process.exit(1);
  }
}

module.exports = {connectDB};
```

## 3. app.js | Server | Routes

```js
const express = require("express");

const { connectDB } = require("./database.js");
const { User } = require('./models/user.js');
const { Country } = require("./models/countries.js");

const app = express();
const PORT = 3000;

app.use(express.json());

// User Routes
app.post("/users", async (req, res) => {
  try {
    // Accepts either a single user object or an array of user objects
    if (Array.isArray(req.body)) {
      await User.insertMany(req.body);
    } else {
      const user = new User(req.body);
      await user.save();
    }
    res.status(201).json({ message: "User(s) saved successfully" });
  } catch (error) {
    console.error(error);
    res.status(400).json({ message: "Failed to save the data" });
  }
});

app.get("/users", async (req, res) => {
  try {
    // Optimized: Check document count first instead of fetching all user records into memory
    const totalCount = await User.countDocuments();
    if (totalCount === 0) {
      return res.status(404).json({ message: "User not found" });
    }

    // Run specific queries concurrently for better performance using Promise.all
    const [limitUsers, sortUsers, selectUsers, whereUsers] = await Promise.all([
      User.find().limit(2),
      User.find().limit(2).sort({ age: 1 }),
      User.find().limit(2).sort({ age: 1 }).select({ login: true, password: true }),
      User.find().where('age').gt(18).lt(32).limit(3).sort({ age: 1 })
    ]);
    
    res.json({ limitUsers, sortUsers, selectUsers, whereUsers });
  } catch (error) {
    console.error(error);
    res.status(400).json({ message: "Failed to get users" });
  }
});

// COUNTRY ROUTES & SEEDING 

const countriesData = [
    {country: 'USA', capital: 'Washington', area: 9833520, population: 327167434, currency: 'USD'},
    {country: 'Italy', capital: 'Rome', area: 301340, population: 60483973, currency: 'EUR'},
    {country: 'Germany', capital: 'Berlin', area: 357386, population: 83000000, currency: 'EUR'},
    {country: 'Canada', capital: 'Ottawa', area: 3855100, population: 37242571, currency: 'CAD'},
    {country: 'China', capital: 'Beijing', area: 9596961, population: 1403500365, currency: 'CNY'},
    {country: 'Sweden', capital: 'Stockholm', area: 450295, population: 10223505, currency: 'SEK'},
    {country: 'India', capital: 'New Delhi', area: 3287263, population: 1324171354, currency: 'INR'},
    {country: 'Netherlands', capital: 'Amsterdam', area: 41543, population: 17302139, currency: 'EUR'}
];

app.get("/countries", async (req, res) => {
  try {
    let countries = await Country.find()
                                 .where('area').gt(100000)
                                 .where('currency').equals("EUR")
                                 .sort({ country: 1 })
                                 .select({ country: true, area: true });
                                 
    if (countries.length === 0) {
        return res.status(404).json({ message: "Country not found" });
    }
    res.json(countries);                         
  } catch (error) {
    console.error(error);
    res.status(400).json({ error: "Failed to get countries" });
  }
});

// DATABASE CONNECTION & SERVER START

connectDB().then(async () => {
  console.log("DB Connection established...");

  // Seed countries safely only if collection is empty
  try {
    const countryCount = await Country.countDocuments();
    if (countryCount === 0) {
      await Country.insertMany(countriesData);
      console.log("Countries seeded successfully!");
    }
  } catch (seedError) {
    console.error("Seeding error:", seedError);
  }

  app.listen(PORT, () => {
    console.log(`Server is running on http://localhost:${PORT}`);
  });
});

```

