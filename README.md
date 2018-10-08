# node-postgresql
Node 操作 postgresql

```js
const { Pool, Client } = require('pg');
const uuid = require('node-uuid');

let pool;
class pgUtil {

	constructor(){
		if(!pool){
			const pgConfig = {
			  user: 'postgres',
			  host: '192.168.0.163',
			  database: 'user',
			  password: 'postgres',
			  port: 5432,
			  max:20,
			  idleTimeoutMillis:3000,
			}

			pool = new Pool(pgConfig);
		}
	}

	/**
	 * rowCount
	 * rows
	 */
	async queryAll() {
		let msg = await pool.query(`SELECT * FROM member`);
		return msg;
	}

	/**
	 * 条件查询
	 */
	async queryItem(username){
		let msg = await pool.query(`SELECT * FROM member WHERE username = $1`,[username]);
		return msg; 
	}

	/**
	 * insert
	 */
	async insert(obj){
		let {username, pass} = obj;
		let uid = uuid.v4();
		let msg = await pool.query(`INSERT INTO member (uid, username, pass) VALUES ($1, $2, $3)`, [uid, username, pass]);
		return msg;
	}

	/**
	 * update
	 */
	async update(pass, username){
		let msg = await pool.query(`UPDATE member SET pass = $1 WHERE username = $2`,[pass, username]);
		return msg; 
	}

	/**
	 * delete
	 */
	async delete(username){
		let msg = await pool.query(`DELETE FROM member WHERE username = $1`,[ username ]);
		return msg; 
	}

	/**
	 * exec
	 */
	async exec(){

		// let msg = await this.queryAll();
		// console.log(msg.rowCount);
		// console.log(msg.rows);


		// let msg = await this.queryItem('adley');
		// console.log(msg.rowCount);
		// console.log(msg.rows);

		// let msg1 = await this.insert({
		// 	username: 'adley',
		// 	pass:'Adley1234.123'
		// });
		// console.log(msg1);

		// let msg = await this.update('111222333', 'adley');
		// console.log(msg.rowCount);
		// console.log(msg.rows);

		let msg = await this.delete('adley');
		console.log(msg.rowCount);
		console.log(msg.rows);

	}

}

new pgUtil().exec();

```
