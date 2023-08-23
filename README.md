## how to add foreign key in mirgations file exmaple

```js 
module.exports = {
  up: async (queryInterface, Sequelize) => {
    await queryInterface.addColumn('Posts', 'userId', {
      type: Sequelize.INTEGER,
      references: {
        model: 'Users', // Target table name
        key: 'id', // Target column name
      },
      onUpdate: 'CASCADE',
      onDelete: 'SET NULL', // or 'CASCADE' or other options
    });
  },

  down: async (queryInterface, Sequelize) => {
    await queryInterface.removeColumn('Posts', 'userId');
  },
};

```

### Undo migrations files

```bash
npx sequelize-cli db:migrate:undo
```


### Foreign key realtionship 

```js
const { DataTypes } = require('sequelize');
const sequelize = require('../config/db'); // Import your Sequelize instance

const User = sequelize.define('User', {
  username: DataTypes.STRING,
  // Other attributes...
});

// Define the association between User and Post models
User.associate = (models) => {
  User.hasMany(models.Post, { foreignKey: 'userId' });
};

module.exports = User;

```


