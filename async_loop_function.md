
async for loop function example

´´´
/**
 * Helper function to get the user data from the team databases.
 * Data is returned only from teams that the user has any privileges in.
 *  
 * @param  {integer} user_id user id from the user table in management database
 * @param  {array} groups list of team objects the user has privileges in
 * @return  {array} groupData array, contains group specific data of the user
 */
async function getUserGroupData(user_id, groups) {
    let groupData = [];


    for (let i = 0; i < groups.length; i++) {

        groupData.push({
            ...await new Promise((resolve) => {


                let dbh = require("../helpers/createKnex")(groups[i].database_name)

                let RuleSiteAdmin = require('../models/Teams/RuleSiteAdmin/RuleSiteAdmin.model');
                let tUser = require("../models/Teams/User/User.model");

                tUser.getUser(dbh)
                    .where({
                        u_id: user_id,
                        deleted: false
                    })
                    .fetch()
                    .then(resultUser => {
                        RuleSiteAdmin.getRuleSiteAdmin(dbh)
                            .where({
                                user_id: resultUser.attributes.id,
                                deleted: false
                            })
                            .fetch({
                                withRelated: ['user', 'siteAdmin']
                            })
                            .then(result => {

                                if (result) {
                                    let tmpUser = result.serialize().user
                                    let tmpSiteAdmin = result.serialize().siteAdmin
                                    if (tmpSiteAdmin.deleted || tmpUser.deleted) {

                                        resolve(null)
                                    }

                                    let tmp = {

                                        groupId: groups[i].id,
                                        groupName: groups[i].name,
                                        groupRole:tmpSiteAdmin.name,
                                        groupACL: tmpSiteAdmin.acl_name,
                                        groupUserPrefStartTime: tmpUser.preferred_start_time,
                                        groupUserPrefEndTime: tmpUser.preferred_end_time,
                                        groupUserCalendarColor: tmpUser.calendar_color,

                                    }

                                    resolve(tmp)

                                } else {

                                    resolve()

                                }


                            })
                            .catch(err => {

                                console.log(err)
                                resolve(null)

                            })


                    }).catch(err => {

                        console.log(err)
                        resolve(null)

                    })



            })

        })

    }
    return groupData.filter(value => Object.keys(value).length !== 0);
}
```
