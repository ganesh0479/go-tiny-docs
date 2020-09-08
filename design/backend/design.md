> # Design
## INTRODUCTION
    This page is under construction
## API Specifications

> ### CREATE USER

    Requirement     : Should create user
    End Point       : /api/v1/url-shortner/users
    Request Type    : POST
    Payload         : {
                          Name         : String,
                          EmailId      : String,
                          Password     : String,
                          CreatedOn    : String
                       }
    Response        : OK

> ### SIGN IN

    Requirement     : User should be able to login
    End Point       : /api/v1/url-shortner/users/login
    Request Type    : POST
    Payload         : {
                          EmailId      : String,
                          Password     : String
                       }
    Response        : OK

> ### CREATE GROUP

    Requirement     : User should be able to create group and creator of group will be the admin
    End Point       : /api/v1/url-shortner/groups
    Request Type    : POST
    Payload         : {
                          Name         : String,
                          CreatedBy    : String (Email ID)
                       }
    Response        : OK
    
> ### ADD/UPDATE ADMIN/USER TO GROUP
    
    Requirement     : Admin user should be able add admin to group
    Validations     : Only admin user should be able to add admin
    End Point       : /api/v1/url-shortner/groups/roles
    Request Type    : POST
    Payload         : {
                          GroupName    : String,
                          RoleName     : ENUM(ADMIN,USER)
                          UserName     : String, (Email ID)
                          AddedBy      : String  (Email ID)
                       }
    Response        : OK

> ### CREATE CARD
    
    Requirement     : User should be able to create card
    End Point       : /api/v1/url-shortner/cards
    Request Type    : POST
    Payload         : {
                           Title        : String,
                           Description  : String,
                           Picture      : Multipart,
                           Name         : String,
                           LongURL      : String,
                           ExpiresIn    : Integer,
                           CreatedBy    : String
                       }
    Response        : {
                            TinyURL : String,
                            Name    : String
                       }

> ### UPDATE CARD
    
    Requirement     : User should be able to update card
    End Point       : /api/v1/url-shortner/cards/{CARD_NAME}
    Request Type    : PATCH
    Payload         : {
                           Title        : String
                           Description  : String,
                           Picture      : Multipart,
                           Name         : String,                           
                           LongURL      : String,
                           ExpiresIn    : Integer,
                           CreatedBy    : String
                       }
    Response        : OK
    
    
> ### DELETE CARD
    
    Requirement     : User should be able to delete card
    End Point       : /api/v1/url-shortner/cards/{CARD_NAME}
    Request Type    : DELETE
    Response        : OK
    
> ### GET CARD
    
    Requirement     : User should be able to get card
    End Point       : /api/v1/url-shortner/cards/{CARD_NAME}
    Request Type    : GET
    Response        : {
                           Title        : String
                           Description  : String,
                           Picture      : Multipart,
                           Name         : String,                           
                           LongURL      : String,
                           ExpiresIn    : Integer,
                           GroupName    : String
                       }
                       
> ### ADD CARD TO THE GROUP
    
    Requirement     : Admin user should be able to authorize or add card to group and remove expiry time of the card
    End Point       : /api/v1/url-shortner/groups/{GROUP_NAME}/cards
    Request Type    : POST
    Payload         : {
                          CardName     : String,
                          AddedBy      : String,
                       }
    Response        : OK
    
> ### AUTHORIZE CARD IN THE GROUP
    
    Requirement     : Admin user should be able to authorize card to group and remove expiry time of the card
    End Point       : /api/v1/url-shortner/groups/{GROUP_NAME}/cards/{CARD_NAME}/authorize
    Request Type    : GET
    Response        : OK  
    
> ### APPROVE CARD IN THE GROUP
    
    Requirement     : Admin user should be able to authorize card to group and remove expiry time of the card
    End Point       : /api/v1/url-shortner/groups/{GROUP_NAME}/cards/{CARD_NAME}/approve
    Request Type    : GET
    Response        : OK  
    
> ### FETCH CARDS NOT BELONGS TO GROUP

    Requirement     : User should be able to get cards
    End Point       : /api/v1/url-shortner/cards
    Request Type    : GET
    Response        : {
                        [
                         {
                           Title        : String,
                           Description  : String,
                           Picture      : Multipart,
                           Name         : String,
                           LongURL      : String,
                           CreatedBy    : String
                         }
                        ]
                       }
                       
> ### FETCH CARDS BELONGS TO GROUP

    Requirement     : User should be able to get cards
    End Point       : /api/v1/url-shortner/cards/{GROUP_NAME}
    Request Type    : GET
    Response        : {
                        [
                         {
                           Title        : String,
                           Description  : String,
                           Picture      : Multipart,
                           Name         : String,
                           LongURL      : String,
                           CreatedBy    : String,
                           GrpAuthStatus: String
                         }
                        ]
                       }
                       
> ### FETCH CARDS BELONGS TO GROUP BY STATUS

    Requirement     : User should be able to get cards
    End Point       : /api/v1/url-shortner/cards/{GROUP_NAME}/{STATUS}
    Request Type    : GET
    Response        : {
                        [
                         {
                           Title        : String,
                           Description  : String,
                           Picture      : Multipart,
                           Name         : String,
                           LongURL      : String,
                           CreatedBy    : String,
                           GrpAuthStatus: String
                         }
                        ]
                       }    
                                            
> ### UPDATE CARD IN THE GROUP
    
    Requirement     : User should be able to update card
    End Point       : /api/v1/url-shortner/cards/{CARD_NAME}/{GROUP_NAME}
    Request Type    : PATCH
    Payload         : {
                           Title        : String
                           Description  : String,
                           Picture      : Multipart,
                           Name         : String,                           
                           LongURL      : String,
                           ExpiresIn    : Integer,
                           CreatedBy    : String
                       }
    Response        : {
                        Status : SENT_FOR_APPROVAL}
                      }
    
> ### DELETE CARD
    
    Requirement     : User should be able to delete card
    End Point       : /api/v1/url-shortner/cards/{CARD_NAME}/{GROUP_NAME}
    Request Type    : DELETE
    Response        : {
                        Status : SENT_FOR_APPROVAL}
                      }

> ### RESOLVE TINY
    
    Requirement     : User should be able to actual url with tiny url
    End Point       : /
    Request Type    : GET
    Response        : Resolved url or error page