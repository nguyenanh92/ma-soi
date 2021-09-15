<template>
  <div>
    <div class="col-lg-8 mx-auto p-3 py-md-5">
      <header class="d-flex align-items-center pb-3 mb-5 border-bottom">
        <a
          href="/"
          class="d-flex align-items-center text-dark text-decoration-none"
        >
          <img
            src="../assets/brand/fa99695d46c1e28ef267519904f8dcb5.png"
            width="40"
          />
          <span class="fs-4"><b>MA SÓI</b> - SÓI NON HAM ĂN </span>
        </a>
      </header>

      <main>
        <div class="row g-5">
          <div class="col-md-6">
            <h4>Members :</h4>
            <br />
            <ul
              class="list-group"
              v-for="(item, index) in thisListMember"
              :key="item.id"
            >
              <li
                class="
                  list-group-item
                  mt-1
                  d-flex
                  justify-content-between
                  align-items-center
                "
              >
                {{ index + 1 }}
                <b>{{ item.name }}</b>
                <i
                  class="fa fa-times"
                  aria-hidden="true"
                  @click="removeMember(index)"
                >
                </i>
              </li>
            </ul>
            <div class="input-group mb-3 mt-5">
              <input
                type="text"
                class="form-control"
                placeholder="Nhập tên member"
                aria-label="Recipient's username"
                aria-describedby="button-addon2"
                v-model="newMember"
              />
              <button
                class="btn btn-secondary"
                type="button"
                id="button-addon2"
                @click="addMember"
              >
                Thêm member
              </button>
            </div>
          </div>

          <div class="col-md-6">
            <h4>Roles :</h4>
            <br />
            <ul
              class="list-group"
              v-for="(item, index) in thisListRole"
              :key="item.id"
            >
              <li
                class="
                  list-group-item
                  mt-1
                  d-flex
                  justify-content-between
                  align-items-center
                "
              >
                {{ index + 1 }}
                <b>{{ item.role }}</b>
                <i
                  class="fa fa-times"
                  aria-hidden="true"
                  @click="removeRole(index)"
                >
                </i>
              </li>
            </ul>
            <div class="input-group mb-3 mt-5">
              <input
                type="text"
                v-model="newRole"
                class="form-control"
                placeholder="Nhập tên role"
                aria-label="Recipient's username"
                aria-describedby="button-addon2"
              />
              <button
                class="btn btn-secondary"
                type="button"
                id="button-addon2"
                @click="addRole"
              >
                Thêm Role
              </button>
            </div>
          </div>
        </div>
        <hr class="col-3 col-md-2 mb-5" />

        <div class="mb-5">
          <button
            @click="newGame"
            class="btn btn-success btn-lg px-4"
            v-show="!showNewGame"
          >
            Start Game
          </button>
          <button
            @click="resetGame"
            class="btn btn-danger btn-lg px-4"
            v-show="showNewGame"
          >
            Reset Game
          </button>
        </div>
        <br />
        <div class="" v-show="showNewGame" style="">
          <h3>Làng Lá</h3>
          <hr class="col-3 col-md-2 mb-3" />
        </div>
        <table class="table table-hover" v-show="showNewGame">
          <thead>
            <tr>
              <th scope="col">#</th>
              <th scope="col">Tên Member</th>
              <th class="text-center" scope="col">Vai Trò</th>
              <th class="text-center" scope="col">Trạng Thái</th>
              <th class="text-center" scope="col">Ghi Chú</th>
              <th class="text-center" scope="col">Hành động</th>
            </tr>
          </thead>
          <tbody>
            <tr
              v-for="(item, index) in memberAliveStore"
              :key="item.id"
              v-bind:class="{ isGuad: item.guard }"
            >
              <th scope="row">{{ index + 1 }}</th>
              <td>
                <h5>{{ item.name }}</h5>
              </td>
              <td class="text-center">
                <b>{{ item.role }}</b>
              </td>
              <td class="text-center">
                <small><b>Còn Sống</b></small>
              </td>
              <td class="text-center">
                <input
                  v-on:blur="updatNote(item.id, item.note)"
                  v-model="item.note"
                />
              </td>
              <td class="text-center">
                <button
                  type="button"
                  class="btn btn-success btn-sm"
                  @click="actionUser(item.id, 1)"
                >
                  Bảo vệ
                </button>
                <button
                  type="button"
                  class="btn btn-danger btn-sm"
                  @click="actionUser(item.id, 2)"
                >
                  Sói Giết
                </button>
                <button
                  type="button"
                  class="btn btn-primary btn-sm"
                  @click="actionUser(item.id, 3)"
                >
                  Ném Bình
                </button>
                <button
                  type="button"
                  class="btn btn-warning btn-sm"
                  @click="actionUser(item.id, 4)"
                >
                  Treo cổ
                </button>
              </td>
            </tr>
          </tbody>
        </table>
        <br />
        <div class="right" v-show="showNewGame">
          <h3 v-show="showNewGame">Khán Giả</h3>
          <hr class="col-3 col-md-2 mb-3" />
        </div>

        <table class="table table-hover" v-show="showNewGame">
          <thead>
            <tr>
              <th scope="col">#</th>
              <th scope="col">Tên Member</th>
              <th class="text-center" scope="col">Vai Trò</th>
              <th class="text-center" scope="col">Trạng Thái</th>
              <th class="text-center" scope="col">Ghi Chú</th>
              <th class="text-center" scope="col">Hành động</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(item, index) in memberDeadStore" :key="item.id">
              <th scope="row">{{ index + 1 }}</th>
              <td>
                <h5>{{ item.name }}</h5>
              </td>
              <td class="text-center">
                <b>{{ item.role }}</b>
              </td>
              <td class="text-center">
                <small
                  ><b>{{ item.status }}</b></small
                >
              </td>
              <td class="text-center">
                <input
                  v-on:blur="updatNote2(item.id, item.note)"
                  v-model="item.note"
                />
              </td>
              <td class="text-center">
                <button
                  type="button"
                  class="btn btn-success btn-sm"
                  @click="actionViewer(item.id, 1, item.status)"
                >
                  Phùy Thủy Cứu
                </button>
                <button
                  type="button"
                  class="btn btn-dark btn-sm"
                  @click="actionViewer(item.id, 2, null)"
                >
                  Hồi Sinh
                </button>
              </td>
            </tr>
          </tbody>
        </table>
      </main>
      <footer class="pt-5 my-5 text-muted border-top">
        Created by SoulSad &middot; &copy; 2021
      </footer>
    </div>
  </div>
</template>

<script>
export default {
  name: "HelloWorld",
  data() {
    return {
      listMember: [],
      newMember: null,
      newRole: null,
      note: null,
      listRole: [
        { id: this.$uuid.v4(), role: "Sói" },
        { id: this.$uuid.v4(), role: "Sói" },
        { id: this.$uuid.v4(), role: "Dân" },
        { id: this.$uuid.v4(), role: "Dân" },
        { id: this.$uuid.v4(), role: "Phùy Thủy" },
        { id: this.$uuid.v4(), role: "Mẹ Trẻ" },
        { id: this.$uuid.v4(), role: "Bán Sói" },
        { id: this.$uuid.v4(), role: "Tiên Tri" },
        { id: this.$uuid.v4(), role: "Sói Fake" },
      ],
      thisListRole: null,
      thisListMember: [],
      showNewGame: false,
      memberAlive: [],
      memberAliveStore: [],
      listRoleFilter: [],
      memberDead: [],
      memberDeadStore: [],
      binhCuu: 0,
      binhNem: 0,
    };
  },
  created() {
    if (!localStorage.getItem("listRole")) {
      try {
        // this.cats = JSON.parse(localStorage.getItem('listRole'));
        const parsed = JSON.stringify(this.listRole);
        localStorage.setItem("listRole", parsed);
        this.thisListRole = JSON.parse(localStorage.getItem("listRole"));
      } catch (e) {
        localStorage.removeItem("listRole");
      }
    } else {
      this.thisListRole = JSON.parse(localStorage.getItem("listRole"));
    }
    if (!localStorage.getItem("listMember")) {
      try {
        // this.cats = JSON.parse(localStorage.getItem('listRole'));
        const parsed = JSON.stringify(this.listMember);
        localStorage.setItem("listMember", parsed);
      } catch (e) {
        localStorage.removeItem("listMember");
      }
    } else {
      this.thisListMember = JSON.parse(localStorage.getItem("listMember"));
    }

    if (localStorage.getItem("memberLive")) {
      this.showNewGame = !this.showNewGame;
      this.memberAliveStore = JSON.parse(localStorage.getItem("memberLive"));
    }
    if (localStorage.getItem("memberDead")) {
      this.memberDeadStore = JSON.parse(localStorage.getItem("memberDead"));
    }

    if (localStorage.getItem("binhCuu")) {
      this.binhCuu = JSON.parse(localStorage.getItem("binhCuu"));
    }

    if (localStorage.getItem("binhNem")) {
      this.binhNem = JSON.parse(localStorage.getItem("binhNem"));
    }
  },
  methods: {
    addRole() {
      // ensure they actually typed something
      if (!this.newRole) {
        return;
      }
      // this.cats.push(this.newCat);
      // let id = Math.floor(Math.random() * 10000);

      let newItem = {
        id: this.$uuid.v4(),
        role: this.newRole,
      };
      this.thisListRole.push(newItem);

      this.saveRole();
    },
    removeRole(x) {
      this.thisListRole.splice(x, 1);
      this.saveRole();
    },
    saveRole() {
      const parsed = JSON.stringify(this.thisListRole);
      localStorage.setItem("listRole", parsed);
      this.newRole = "";
    },
    addMember() {
      // ensure they actually typed something
      if (!this.newMember) {
        return;
      }
      // this.cats.push(this.newCat);
      // let id = Math.floor(Math.random() * 10000);

      let newItem = {
        id: this.$uuid.v4(),
        name: this.newMember,
      };
      this.thisListMember.push(newItem);

      this.saveMember();
    },
    removeMember(x) {
      this.thisListMember.splice(x, 1);
      this.saveMember();
    },
    saveMember() {
      const parsed = JSON.stringify(this.thisListMember);
      localStorage.setItem("listMember", parsed);
      this.newMember = "";
    },
    newGame() {
      const self = this;
      const countMember = this.thisListMember.length;
      const countRole = this.thisListRole.length;

      if (countMember == countRole) {
        this.showNewGame = !this.showNewGame;
        this.listRoleFilter = JSON.parse(
          JSON.stringify(this.thisListRole.map((a) => a.role))
        );

        this.thisListMember.map((x) => {
          // let id = Math.floor(Math.random() * 10000);

          this.memberAlive.push({
            id: this.$uuid.v4(),
            name: x.name,
            role: self.random_item(self.listRoleFilter),
            dead: false,
            guard: false,
            note: null,
            status: null,
          });
        });

        const parsed = JSON.stringify(this.memberAlive);
        localStorage.setItem("memberLive", parsed);
        this.memberAliveStore = JSON.parse(localStorage.getItem("memberLive"));

        if (!localStorage.getItem("binhCuu")) {
          localStorage.setItem("binhCuu", 1);
          this.binhCuu = JSON.parse(localStorage.getItem("binhCuu"));
        }

        if (!localStorage.getItem("binhNem")) {
          localStorage.setItem("binhNem", 1);
          this.binhNem = JSON.parse(localStorage.getItem("binhNem"));
        }
      } else {
        this.$swal.fire({
          icon: "warning",
          title: "Số lượng member và role phải cân bằng!",
          showDenyButton: false,
          showCancelButton: false,
          confirmButtonText: "OK",
        });
      }
    },
    random_item(items) {
      var index = Math.floor(Math.random() * items.length);
      var item = items[index];
      this.listRoleFilter.splice(index, 1);
      return item;
    },
    resetGame() {
      this.$swal
        .fire({
          title: "Reset game sẽ kết thúc game hiện tại?",
          showDenyButton: true,
          showCancelButton: false,
          confirmButtonText: "Reset",
          denyButtonText: `Không reset`,
        })
        .then((result) => {
          /* Read more about isConfirmed, isDenied below */
          if (result.isConfirmed) {
            this.memberAlive = [];
            this.memberDead = [];
            this.memberDeadStore = [];
            this.showNewGame = !this.showNewGame;
            localStorage.removeItem("memberLive");
            localStorage.removeItem("memberDead");
            localStorage.removeItem("binhCuu");
            localStorage.removeItem("binhNem");
          } else if (result.isDenied) {
            console.log("Không reset!");
          }
        });
    },
    updatNote(id, text) {
      let data = this.memberAliveStore.map((item, index) => {
        if (item.id == id) {
          item.note = text;
        }
        return item;
      });
      const parsed = JSON.stringify(data);
      localStorage.setItem("memberLive", parsed);
      this.memberAliveStore = JSON.parse(localStorage.getItem("memberLive"));
    },
    updatNote2(id, text) {
      let data = this.memberDeadStore.map((item, index) => {
        if (item.id == id) {
          item.note = text;
        }
        return item;
      });
      const parsed = JSON.stringify(data);
      localStorage.setItem("memberDead", parsed);
      this.memberDeadStore = JSON.parse(localStorage.getItem("memberDead"));
    },
    actionUser(id, type) {
      if (type == 1) {
        let data = this.memberAliveStore.map((item, index) => {
          if (item.id == id) {
            item.guard = !item.guard;
          } else {
            item.guard = false;
          }
          return item;
        });
        const parsed = JSON.stringify(data);
        localStorage.setItem("memberLive", parsed);
        this.memberAliveStore = JSON.parse(localStorage.getItem("memberLive"));
      }
      if (type == 2) {
        var checkGuard = -1;
        let data = this.memberAliveStore.map((item, index) => {
          if (item.id == id && item.guard == false) {
            item.dead = true;
            item.status = "Sói Giết";
            checkGuard = 0;
          }
          return item;
        });

        if (checkGuard == 0) {
          const alive = JSON.stringify(data.filter((a) => a.dead === false));
          localStorage.setItem("memberLive", alive);
          this.memberAliveStore = JSON.parse(
            localStorage.getItem("memberLive")
          );

          if (localStorage.getItem("memberDead")) {
            this.memberDead = JSON.parse(localStorage.getItem("memberDead"));
          }

          const dead = data.map((item, index) => {
            if (item.dead == true) {
              this.memberDead.unshift(item);
            }
          });

          const lstDead = JSON.stringify(...this.memberDead);

          localStorage.setItem("memberDead", ...lstDead);
          this.memberDeadStore = JSON.parse(localStorage.getItem("memberDead"));
        } else {
          this.$swal.fire({
            icon: "info",
            title: "Người này đang được bảo vệ!",
            showDenyButton: false,
            showCancelButton: false,
            confirmButtonText: "OK",
          });
        }
      }
      if (type == 3) {
        if (this.binhNem === 0) {
          this.$swal.fire({
            icon: "info",
            title: "Đã hết quyền ném bình!",
            showDenyButton: false,
            showCancelButton: false,
            confirmButtonText: "OK",
          });
        } else {
          if (localStorage.getItem("binhNem")) {
            localStorage.setItem("binhNem", 0);
            this.binhNem = JSON.parse(localStorage.getItem("binhNem"));
          }

          let data = this.memberAliveStore.map((item, index) => {
            if (item.id == id) {
              item.dead = true;
              item.status = "Phù thủy ném bình";
            }
            return item;
          });

          const alive = JSON.stringify(data.filter((a) => a.dead === false));
          localStorage.setItem("memberLive", alive);
          this.memberAliveStore = JSON.parse(
            localStorage.getItem("memberLive")
          );

          if (localStorage.getItem("memberDead")) {
            this.memberDead = JSON.parse(localStorage.getItem("memberDead"));
          }

          const dead = data.map((item, index) => {
            if (item.dead == true) {
              this.memberDead.unshift(item);
            }
          });

          const lstDead = JSON.stringify(this.memberDead);
          localStorage.setItem("memberDead", lstDead);
          this.memberDeadStore = JSON.parse(localStorage.getItem("memberDead"));
        }
      }
      if (type == 4) {
        let data = this.memberAliveStore.map((item, index) => {
          if (item.id == id) {
            item.dead = true;
            item.status = "Dân làng treo cổ";
          }
          return item;
        });

        const alive = JSON.stringify(data.filter((a) => a.dead === false));
        localStorage.setItem("memberLive", alive);
        this.memberAliveStore = JSON.parse(localStorage.getItem("memberLive"));

        if (localStorage.getItem("memberDead")) {
          this.memberDead = JSON.parse(localStorage.getItem("memberDead"));
        }

        const dead = data.map((item, index) => {
          if (item.dead == true) {
            this.memberDead.unshift(item);
          }
        });

        const lstDead = JSON.stringify(this.memberDead);
        localStorage.setItem("memberDead", lstDead);
        this.memberDeadStore = JSON.parse(localStorage.getItem("memberDead"));
      }
    },
    actionViewer(id, type, status) {
      if (type == 1) {
        if (this.binhCuu === 0) {
          this.$swal.fire({
            icon: "info",
            title: "Đã hết quyền cứu!",
            showDenyButton: false,
            showCancelButton: false,
            confirmButtonText: "OK",
          });
        } else {
          if (status === "Phù thủy ném bình") {
            this.$swal.fire({
              icon: "info",
              title: "Khong thể cứu vì bị ném bình!",
              showDenyButton: false,
              showCancelButton: false,
              confirmButtonText: "OK",
            });
          } else {
            if (localStorage.getItem("binhCuu")) {
              localStorage.setItem("binhCuu", 0);
              this.binhCuu = JSON.parse(localStorage.getItem("binhCuu"));
            }

            let data = this.memberDeadStore.map((item, index) => {
              if (item.id == id) {
                item.dead = false;
                item.status = "Phù thủy cứu";
              }
              return item;
            });

            const dead = JSON.stringify(data.filter((a) => a.dead === true));
            localStorage.setItem("memberDead", dead);
            this.memberDeadStore = JSON.parse(
              localStorage.getItem("memberDead")
            );

            if (localStorage.getItem("memberLive")) {
              this.memberAlive = JSON.parse(localStorage.getItem("memberLive"));
            }

            const alive = data.map((item, index) => {
              if (item.dead == false) {
                this.memberAlive.unshift(item);
              }
            });

            console.log(this.memberAlive);

            const lstAlive = JSON.stringify(this.memberAlive);
            localStorage.setItem("memberLive", lstAlive);
            this.memberAliveStore = JSON.parse(
              localStorage.getItem("memberLive")
            );
          }
        }
      }

      if (type == 2) {
        let data = this.memberDeadStore.map((item, index) => {
          if (item.id == id) {
            item.dead = false;
            item.status = "Hồi Sinh";
          }
          return item;
        });

        const dead = JSON.stringify(data.filter((a) => a.dead === true));
        localStorage.setItem("memberDead", dead);
        this.memberDeadStore = JSON.parse(localStorage.getItem("memberDead"));

        if (localStorage.getItem("memberLive")) {
          this.memberAlive = JSON.parse(localStorage.getItem("memberLive"));
        }

        const alive = data.map((item, index) => {
          if (item.dead == false) {
            this.memberAlive.unshift(item);
          }
        });

        console.log(this.memberAlive);

        const lstAlive = JSON.stringify(this.memberAlive);
        localStorage.setItem("memberLive", lstAlive);
        this.memberAliveStore = JSON.parse(localStorage.getItem("memberLive"));
      }
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.bd-placeholder-img {
  font-size: 1.125rem;
  text-anchor: middle;
  -webkit-user-select: none;
  -moz-user-select: none;
  user-select: none;
}

@media (min-width: 768px) {
  .bd-placeholder-img-lg {
    font-size: 3.5rem;
  }
}
.isGuad {
  background-color: #a6ea3e;
}
i:hover {
  cursor: pointer;
}
</style>
